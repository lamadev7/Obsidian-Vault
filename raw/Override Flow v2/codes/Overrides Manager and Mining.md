---
type: code-note
domain: Override Flow
source: portpro-ai-agents/app/routes/messaging/override_review.py, app/services/messaging/message_service.py, app/agents/overrides_manager/*, app/routes/playbook_learning.py
updated: 2026-06-20
---

# Overrides Manager and Mining — code walk

> Explains: [[Override Flow v2]] · [[User Flow]] steps 4–6 · [[Technical Reference]] pillars 3–4.
> Covers the ledger log + card post, the claim gate, the explain-only agent, classification, the
> kind registry, the auto-dispatched mine tools, and the patch synthesis worker. Code verbatim from
> `origin/feat/override-v2`.

## Log the capture as "observed"

```python
# portpro-ai-agents/app/routes/messaging/override_review.py:110-148
        "metadata": {
            "override_kind": req.override_kind,
            "action": req.action,
            "reason": req.reason,
            "actor": req.actor.model_dump() if req.actor else None,
            "reference_number": req.reference_number,
            "load_id": req.load_id,
            "customer_id": req.customer_id,
            "entity": req.entity,
            "snapshot": req.snapshot,
            "occurred_at": req.occurred_at,
            "review_status": "observed",
        },
```
**The ledger row** (ai-request-logs, `category=manual_override`). `review_status:"observed"` is the
v2 marker — the override is already applied and never awaits a decision. This row is the join table
the billing surface and the card both read.

```python
# portpro-ai-agents/app/routes/messaging/override_review.py:198-233
        exception_data: Dict[str, Any] = {
            "override_kind": body.override_kind, "action": body.action, "reason": body.reason,
            "actor": body.actor.model_dump() if body.actor else None,
            "reference_number": body.reference_number, "load_id": body.load_id,
            "customer_id": body.customer_id, "entity": body.entity,
            "exception_id": log_id, "occurred_at": body.occurred_at,
            "review_status": "observed",
        }
        service = AgentMessagingService()
        result = await service.post_override_review(
            carrier_id=body.carrier_id, channel_name=OVERRIDE_REVIEW_CHANNEL_NAME,
            agent_id=OVERRIDE_REVIEW_AGENT_ID, content=content,
            exception_type=OVERRIDE_REVIEW_EXCEPTION_TYPE, exception_data=exception_data,
            exception_id=log_id, load_id=body.load_id, customer_id=body.customer_id,
            actor_user_id=body.actor.user_id if body.actor else None)
```
**Posts the `override_review` card** to the carrier's needs-review channel with the same `observed`
status, linking card ↔ log via `exception_id`.

## Claim gate (dupe-run guard)

```python
# portpro-ai-agents/app/services/messaging/message_service.py:538-561
    async def begin_override_verification(self, message_id: UUID) -> bool:
        status = await self.db.execute_query(
            """
            UPDATE channel_messages
            SET exception_data = jsonb_set(COALESCE(exception_data,'{}'::jsonb),'{review_status}','"in_discussion"'),
                updated_at = NOW()
            WHERE id = $1 AND is_deleted = FALSE AND exception_type = 'override_review'
              AND COALESCE(exception_data->>'review_status','pending_review') IN ('pending_review','observed')
            """, message_id)
        return self._parse_rowcount(status) == 1
```
**Atomic claim.** The single-row `UPDATE` is the dupe guard — only one analysis run can flip the card
from `observed`/`pending_review` to `in_discussion`. **Critical v2 fix:** the WHERE clause had to
add `'observed'`; without it, v2's observed cards were never picked up and never analyzed.
`finish_override_verification` returns the card to `observed` after the run (only from
`in_discussion`) so the badge doesn't read "Under AI review" forever.

## Explain-only charter

```python
# portpro-ai-agents/app/agents/overrides_manager/prompt.py:1-16
"""
Overrides Manager — system prompt (v2, observe mode).
The agent receives an ALREADY-APPLIED manual override ... The agent's job is EXPLAIN-ONLY:
  1. Gather the evidence sources (tariff, playbook, history, rate computation, AI rationale)
     with READ-ONLY tools.
  2. Post a short, plain-prose FACTUAL ANALYSIS ... with NO recommendation, NO verdict, and
     NO "confirm?" question.
  3. In the SAME run, classify the override's sop_type and AUTOMATICALLY call exactly ONE
     native mine tool (mine_from_override_contract for a tariff-backed charge,
     mine_from_override for everything else).
"""
```
**The agent's whole job in three lines:** gather evidence (read-only), explain (prose), classify +
mine (one tool). No apply, no recommend.

```python
# portpro-ai-agents/app/agents/overrides_manager/prompt.py:153-176
# Classify and AUTO-DISPATCH the mine tool (SAME run, exactly ONE call)
- **sop_type = contract** → `mine_from_override_contract` — ONLY when `override_kind` is
  charge AND the charge is TARIFF-BACKED (tariffBacked:true / present in get-tariff-for-load).
- **sop_type = playbook** → `mine_from_override` — for EVERYTHING else (non-tariff charge,
  chargeset APPROVE/UNAPPROVE, all document overrides).
Call with EXACTLY these values from context — never invented:
- request_log_id = the override request_log_id / exception_id ; customer_id = the customer_id.
- Exactly ONE mine call per run — never both, never twice.
```
**Classification rule, pinned mechanically.** Tariff-backed charge → contract; all else → playbook.
The "never invented" instruction matters — ids come from context, not the model (see the tool below
for the enforcement).

## Mine tools — session-derived ids

```python
# portpro-ai-agents/app/agents/overrides_manager/tools.py:78-157 (abridged)
async def _kick_override_patch_synthesis(request_log_id, customer_id, channel_id, tool_context, sop_type_target):
    if tool_context is None or getattr(tool_context, "state", None) is None:
        return {"success": False, "error": "No session context (carrier_id unavailable)."}
    carrier_id = tool_context.state.get("carrier_id")
    if not carrier_id:
        return {"success": False, "error": "carrier_id missing from session."}
    if not settings.PLAYBOOK_LEARNING_OVERRIDE_SOURCE_ENABLED:
        return {"success": False, "error": "Playbook sync from overrides is disabled."}
    if not request_log_id or not ObjectId.is_valid(str(request_log_id)):
        return {"success": False, "error": "Invalid request_log_id (not a 24-hex ObjectId)."}
    from app.routes.playbook_learning import _synthesize_override_patch
    thread_id = (tool_context.state.get("thread_parent_message_id") or "").strip() or None
    effective_channel_id = None if thread_id else (str(channel_id) if channel_id else None)
    asyncio.create_task(_synthesize_override_patch(
        request_log_id=str(request_log_id), carrier_id=str(carrier_id), customer_id=str(customer_id),
        body_channel_id=effective_channel_id, body_thread_id=thread_id, sop_type_target=sop_type_target))
```
**The shared dispatcher both mine tools call.** `carrier_id` and `thread_parent_message_id` come
from the **session state**, not the model — this kills the wrong-id hallucinations that plagued v1.
ids are ObjectId-validated, the feature flag is checked, and synthesis is fired in-process
(`asyncio.create_task`, fire-and-forget). The two public tools are thin arms over this:
`mine_from_override` (`sop_type_target="playbook"`) and `mine_from_override_contract`
(`sop_type_target="contract"`).

## Kind registry (extensibility spine)

```python
# portpro-ai-agents/app/agents/overrides_manager/kinds.py:50-72
OVERRIDE_KIND_REGISTRY = {
    "charge": {
        "events": ["ADD_PRICING","UPDATE_PRICING","REMOVE_PRICING","APPROVE_CHARGE","UNAPPROVE_CHARGE"],
        "read_tools": ["get-tariff-for-load","get-all-tariffs-for-load","get-tariff-summary",
            "get-tariff-summary-by-customer","get-charge-templates","get-charge-template-by-id",
            "get-charge-code-list","calculate-doe-surcharge","compute-charge-fields",
            "get-charge-detail","get-charge-details-reference-number"],
        "action_tools": [],
        "present": "computed per-unit RATE explanation via compute-charge-fields",
    },
    "document": {
        "events": ["VALIDATE_DOCUMENT","INVALIDATE_DOCUMENT","REMOVE_DOCUMENT"],
        "read_tools": ["get-combined-document","get-document-signed-url"],
        "action_tools": [],
        "present": "document headline + validation state + signed preview",
    },
}
```
**Single source of truth** for each override kind → its events + read-only tools. Note `action_tools`
is empty everywhere — the explain-only agent has *no* write tools. Adding a new override kind is one
registry entry; the generic spine (gather → explain → classify → mine) is untouched.

## Synthesis worker (both arms converge)

```python
# portpro-ai-agents/app/routes/playbook_learning.py:1453-1480 (signature)
async def _synthesize_override_patch(*, request_log_id, carrier_id, customer_id,
        body_channel_id, body_thread_id=None, sop_type_target="playbook") -> None:
    """ONE override doc → one patch suggestion.
    sop_type_target: 'playbook' (default) = untyped active sop_embedding + scenario EXPAND;
    'contract' = active sop_type='contract' embedding, draft reads as a negotiated term,
    playbook_scenarios SKIPPED."""
```
**One override doc → one patch suggestion** through Generator → Reviewer → Arbiter. `sop_type_target`
selects the target SOP embedding (playbook vs contract). The worker never raises — a failed mine just
means no patch card. The HTTP `/mine-from-override` route is the legacy/backend trigger; both it and
the native tools converge on this same worker.

## Related
[[Override Flow v2]] · [[Technical Reference]] · [[codes/Backend Validator and Capture]] ·
[[codes/Patch Accept and SOP Routing]]
