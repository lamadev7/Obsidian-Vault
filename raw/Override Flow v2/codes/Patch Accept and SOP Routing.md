---
type: code-note
domain: Override Flow
source: portpro-frontend/.../PlaybookPatchSuggestion/*, portpro-ai-agents/app/agents/comms/classifier.py, app/services/messaging/auto_response_service.py, portpro-frontend/.../ThreadSidebar.jsx
updated: 2026-06-20
---

# Patch Accept and SOP Routing — code walk

> Explains: [[Override Flow v2]] · [[User Flow]] steps 7–8 · [[Technical Reference]] pillar 5.
> Covers the patch widget Accept, the sentinel that routes to the SOP agent, the classifier
> short-circuit, the SOP dispatch plumbing, and the inline `SopRunPanel`. This is the seam where the
> override flow hands off to SOP V3. Code verbatim from `origin/feat/override-v2`.

## The sentinel (FE ↔ ai contract)

The whole hand-off rides on one string constant that **must stay identical** in both repos:

```jsx
// portpro-frontend/.../PlaybookPatchSuggestion/OverridePatchSuggestionThread.jsx:33
const OVERRIDE_PATCH_ACCEPT_SENTINEL = "[[OVERRIDE_PATCH_ACCEPT]]";
```
```python
# portpro-ai-agents/app/agents/comms/classifier.py:45-49
OVERRIDE_PATCH_ACCEPT_SENTINEL = "[[OVERRIDE_PATCH_ACCEPT]]"
```
**Why a sentinel instead of an API call?** An override thread carries *both* playbook_patch and
override_review context, so a normal classify would mis-route Accept to `patch_agent` or
`overrides_manager`. A system-emitted sentinel in the message body is a deterministic guard the
classifier checks first.

## FE — compose + Accept

```jsx
// portpro-frontend/.../PlaybookPatchSuggestion/OverridePatchSuggestionThread.jsx:35-87
const composeSopMessage = (data) => {
    const entityLabel = sopTypeTarget === "contract" ? "contract" : "playbook";
    const action = data?.action || "ADD_RULE";
    const section = data?.section || "(no section)";
    const targetRule = data?.target_rule;
    const newText = data?.new_text || "";
    const rationale = data?.rationale || "";
    const customerLabel = customerName
      ? `${customerName}${customerId ? ` (${customerId})` : ""}` : customerId || "(unknown customer)";
    return (
      `${OVERRIDE_PATCH_ACCEPT_SENTINEL} ` +
      `For customer ${customerLabel}, update the ${entityLabel}: ` +
      `${action} in section "${section}"` +
      (targetRule ? ` replacing rule "${targetRule}"` : "") + `: ${newText}.` +
      (rationale ? ` Rationale: ${rationale}.` : ""));
};

const handleAccept = async (data) => {
    await submitPlaybookPatchDecision({ suggestionId: data.suggestion_id, decision: "route" });
    if (typeof onAcceptRouteToThread === "function") {
      await onAcceptRouteToThread(composeSopMessage(data));
      setRouted(true);
    }
};
```
**Accept does two things:** records a `route` decision (NOT apply — the SOP agent applies, which
prevents a double-apply via the P2 patch applier), then posts a self-contained, sentinel-prefixed
instruction into the thread. The message is human-readable on purpose so the SOP agent can act on it
directly.

```jsx
// portpro-frontend/.../PlaybookPatchSuggestion/index.jsx:49-72
const PlaybookPatchSuggestion = ({ data, onDecision, onAccept, variant = "playbook" }) => {
  const isContract = variant === "contract";
  const submit = async (decision) => {
    if (busy || !data?.suggestion_id) return;
    setBusy(true);
    try {
      if (decision === "accept" && typeof onAccept === "function") {
        await onAccept(data);
        toastr.show("Patch routed to the SOP agent — follow the conversation below.", "success");
        onDecision?.();
        return;
      }
      // ... reject path unchanged: submitPlaybookPatchDecision({decision:"reject"})
```
**`variant` is framing only** ("playbook" | "contract" labels). When `onAccept` is provided (the
override-thread wrapper), Accept delegates to it; the Reject path is unchanged.

## ai — classifier short-circuit

```python
# portpro-ai-agents/app/agents/comms/classifier.py:312-329
    if message and OVERRIDE_PATCH_ACCEPT_SENTINEL in message:
        logger.info("[CaptainClassifier] Short-circuit: override patch-accept sentinel → sop_agent")
        return "sop_agent", 10
```
**Checked FIRST**, before the override_review / playbook_patch short-circuits. The sentinel
deterministically routes the accepted patch to `sop_agent` (SOP V3) with max confidence.

## ai — SOP dispatch plumbing

```python
# portpro-ai-agents/app/services/messaging/auto_response_service.py:689-703
            session_id = agent_session_id or f"thread_{parent_message_id}"
            if session_id.startswith("thread_"):
                session_id = f"channel_{channel_id}_{parent_message_id}"
```
**Session-id normalize.** Thread replies default to `thread_<id>`, which the SOP bridge can't
classify; this rebuilds the canonical `channel_<channel_id>_<parent>` form the bridge expects.

```python
# portpro-ai-agents/app/services/messaging/auto_response_service.py:856-936 (abridged)
            dispatch_meta: Dict[str, Any] = {}
            async for chunk, is_final in runner(..., dispatch_meta=dispatch_meta, **_sop_scope_kwargs):
                ...
            sop_run_id = dispatch_meta.get("sop_run_id")
            sop_customer_id = dispatch_meta.get("sop_customer_id")
            is_sop_streaming_placeholder = bool(sop_run_id) and not (full_response or "").strip()
            ...
            message_data = ChannelMessageCreate(
                content=full_response, content_type=MessageContentType.TEXT,
                parent_message_id=parent_message_id, exception_id=ai_log_id, session_id=session_id,
                sop_run_id=sop_run_id, sop_customer_id=sop_customer_id,
                opik_trace_id=opik_trace_id, opik_project_name=opik_project_name,
                opik_environment=opik_environment)
```
**The actual SOP bridge.** A mutable `dispatch_meta` is passed into the runner (the bridge mutates
it), the run is scoped to the override's bill-to customer, then `sop_run_id` is read back and stamped
onto the posted channel message so the FE knows to mount the run panel. **Watch-list:** this exact
plumbing must exist in all three channel response handlers — thread-reply (shown), mention, and DM.
The thread-reply one was the easily-forgotten path.

## FE — render the SOP run inline

```jsx
// portpro-frontend/.../AIWorkbenchV2/components/messages/ThreadSidebar.jsx:1932-1951
                        typeof reply.sop_run_id === "string" && reply.sop_run_id.trim() ? (
                          <div className="mt-1 min-width-0">
                            <SopRunPanel
                              runId={reply.sop_run_id.trim()}
                              customerId={typeof reply.sop_customer_id === "string" && reply.sop_customer_id.trim()
                                  ? reply.sop_customer_id.trim() : null}
                              conversationId={null}
                              events={Array.isArray(reply.sop_events) ? reply.sop_events : null}
                            />
                          </div>
                        ) :
```
**No chat-in-chat.** A bot reply carrying `sop_run_id` mounts the *same* `SopRunPanel` the main
conversation uses, so the SOP V3 run (its plan stepper) renders inline in the override thread. This
reuse — `sop_agent` already in the Commander roster + `SopRunPanel` already in the thread renderer —
is why Accept needed no new chat surface.

## Related
[[Override Flow v2]] · [[Technical Reference]] · [[codes/Overrides Manager and Mining]] ·
[[codes/Billing Surface and Reject]]
