---
type: code-note
domain: Billing SOP Agent V3
source: portpro-ai-agents/app/routes/exception_recommendation_agent.py
updated: 2026-06-03
---

# Route Dispatch — code walk

> Explains: [[Customer SOP Message Flow]]

```python
# app/routes/exception_recommendation_agent.py:257-263
@router.post("/billing-sop-chat")
async def billing_sop_chat(
    request: Request,
    payload: ExceptionRecommendationRequest,
    background_tasks: BackgroundTasks
):
```
**The single entry point.** Every customer SOP chat message lands here. `payload`
arrives encrypted; the user (and thus `carrier_id`) comes from
`request.state.user`, set by auth middleware — never from the body.

```python
# app/routes/exception_recommendation_agent.py:327-330
if hasattr(payload, 'data') and payload.data:
    from app.utils.encryption import decrypt_sop_payload
    decrypted_data = decrypt_sop_payload(payload.data)
    payload = ExceptionRecommendationRequest(**decrypted_data)
```
**Decrypt first.** The wire body is a single encrypted `data` blob. After this it
is a normal request object with `message`, `customerIds`, `files`, `context`,
`session_id`, `run_id`.

```python
# app/routes/exception_recommendation_agent.py:357-372
if _is_playbook and payload.entity_type and payload.entity_id and payload.sop_type:
    _route_to_v4 = True
...
if _route_to_v4:
    from app.agents.generic_sop_agent.runner import run_generic_sop_agent
```
**V4 routing gate.** Job descriptions and driver/vendor playbooks are handled by a
*different* agent (V4 generic SOP). A customer billing playbook has no such
`sop_type`, so `_route_to_v4` stays false and control falls through to V3.

```python
# app/routes/exception_recommendation_agent.py:601
use_static_cache_pipeline = await _is_static_cache_pipeline_enabled()
```
**One flag flips the cache path.** A single Redis key decides static-cache vs
classic for all SOP-chat traffic. → [[Prompt Caching (Static-Cache)]]

```python
# app/routes/exception_recommendation_agent.py:823-844
if use_static_cache_pipeline:
    from ...runner_v3_static_cache import run_billing_sop_agent_v3_static_cache
    _runner_iter = run_billing_sop_agent_v3_static_cache(
        user_id=user_id, carrier_id=carrier_id,
        files=payload.files,                 # must be forwarded
        message=message, session_id=conversation_id, auth_token=auth_token,
        firebase_session_id=firebase_session_id,  # must be forwarded
        customer_ids=payload.customer_ids, ...
    )
else:
    _runner_iter = run_billing_sop_agent(...)
```
**The dispatch fork — and a known trap.** Two near-identical call sites with ~14
kwargs each. A prior regression (PR #10085) dropped `files=` and
`firebase_session_id=` from the static-cache branch only — so uploads silently
never reached the model ("I don't see any attached file") whenever the flag was
on. Fixed in PR #10306 by restoring parity. Lesson: when editing one branch,
diff it against the sibling; both must forward the same kwargs.

```python
# app/routes/exception_recommendation_agent.py:946
return StreamingResponse(generate(), media_type="text/event-stream", ...)
```
**The HTTP layer only streams.** `run_billing_sop_agent` already spawned the
background task; `generate()` relays Redis events as SSE. Closing the connection
does not stop the agent. → [[Lifecycle and Persistence]]
