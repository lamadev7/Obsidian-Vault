---
type: code-note
domain: Billing SOP Agent V3
source: portpro-ai-agents/app/agents/billing_agent_v2/playbook_tools.py + sop/runner_v3.py
updated: 2026-06-03
---

# Existing-Docs Picker Tool — code walk

> Explains: [[Document Validation and Existing-Docs Picker]]

```python
# app/agents/billing_agent_v2/playbook_tools.py:3029-3033
async def fetch_existing_customer_documents(
    customer_ids: List[str],
    doc_types: List[str],
    tool_context: ToolContext = None,
) -> Dict[str, Any]:
```
**The whole picker needs only three things** — `customer_ids`, `doc_types`, and an
`auth_token` (read from `tool_context.state`). It carries no SOP state, which is
why it is, in principle, reusable outside Flow E. What gates it today is *how it
gets called*, not what it needs.

```python
# app/agents/billing_agent_v2/playbook_tools.py:3097-3104
source_doc_types = stash_doc_types if stash_doc_types else (doc_types or [])
canonical_doc_types, dropped_doc_types = _canonicalize_doc_types(source_doc_types)
...
if not canonical_doc_types:
    return {"success": False, "error": "No system-supported document types ..."}
```
**Doc types are normalized through the `DocumentType` enum.** The LLM has been seen
to pass `POD` / `SCALE_TICKET`; those are mapped to canonical labels (`Proof of
Delivery`, `Scale Ticket`) or dropped. Empty after normalization → error, so there
is no "fetch all types" mode here — a caller wanting "all" must enumerate the enum.

```python
# app/agents/billing_agent_v2/playbook_tools.py:3138-3144
resp = await client.post(
    f"{backend_url}/tms/ai-doc-picker/list-customer-documents",
    json={"customerIds": customer_ids, "docTypes": doc_types, "page": 1, "pageSize": 20},
    headers={"authorization": token},
)
```
**Backend call.** The endpoint lives in `portpro-backend/server/modules/ai-doc-picker/`.
It scopes to the **carrier from the JWT** (never the body), returns documents only
from **completed + billed loads**, and 403s customer-portal users. The response is
grouped by doc type: `columns[docType] = { totalCount, documents[] }` with signed
URLs.

```python
# app/agents/billing_agent_v2/playbook_tools.py:3180-3187
sid = tool_context.session.id if tool_context and tool_context.session else None
if sid:
    await V3ContextStore.set_value(sid, "pending_doc_picker_table", table_payload)
    await V3ContextStore.set_value(sid, "awaiting_doc_picker_choice", {})
```
**Stash, don't emit.** The tool only parks the table under `pending_doc_picker_table`
and clears the awaiting flag. The runner is what turns that into a wire event — so
adding a `mode` flag here (view vs train) is the natural seam for making the picker
view-only when fired standalone.

```python
# app/agents/billing_agent_v2/sop/runner_v3.py:4956-5031 (intercept, condensed)
if not is_resume and not files and message:
    awaiting = await V3ContextStore.get_value(session.id, "awaiting_doc_picker_choice", None)
    ... # fall back to last_doc_picker_args
    if _picker_args_complete(awaiting) and any(re.search(p, user_text) for p in PICKER_INTENT_PATTERNS):
        picker_result = await fetch_existing_customer_documents(
            customer_ids=intercept_customer_ids, doc_types=awaiting["doc_types"], tool_context=ctx)
```
**The deterministic intercept — and the gate.** Before the LLM runs, the runner
checks for a stashed `awaiting_doc_picker_choice` (or the longer-lived
`last_doc_picker_args`) plus a "show existing / list documents / option 2" text
match, and calls the tool directly (the LLM kept hallucinating the tool as
unavailable). This is exactly why the picker only fires reliably *inside* Flow E:
no stash → no intercept. A start/middle/end "fetch documents" capability would add
an intent path that does not depend on this stash.

```python
# app/agents/billing_agent_v2/sop/runner_v3.py:5471-5481 (post-run drain)
pending_table = await V3ContextStore.get_value(session.id, "pending_doc_picker_table", None)
if isinstance(pending_table, dict) and pending_table.get("columns"):
    await append_event(json.dumps({"type": "doc_picker_table", "data": pending_table}),
                       event_type="v3_doc_picker_table")
    await V3ContextStore.set_value(session.id, "pending_doc_picker_table", {})
```
**Emit happens here, unconditionally.** Any run that left a `pending_doc_picker_table`
emits the `doc_picker_table` SSE event and persists it to chat metadata for reload.
Because this drain runs after *any* tool call, an LLM-driven standalone call already
flows through it — the missing pieces for "anytime" are intent un-gating, customer
resolution, and a view-only mode, not new emit plumbing.
