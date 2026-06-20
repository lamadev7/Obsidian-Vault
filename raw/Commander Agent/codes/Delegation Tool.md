---
type: code-note
domain: Commander Agent
source: portpro-ai-agents/app/agents/comms/delegation_tool.py
updated: 2026-06-10
---

# Delegation Tool — code walk

> Explains: [[Delegation to Sub-Agents]] · [[Override Review Integration]]

```python
# portpro-ai-agents/app/agents/comms/delegation_tool.py:42-50
async def delegate_to_pipeline(
    task_description: str,
    tool_context: ToolContext = None,
) -> str:
    """
    Delegate a WRITE operation to the billing or CSR pipeline for execution.

    Call this ONLY for operations that CREATE, UPDATE, DELETE, or MODIFY data.
    Do NOT call this for read-only queries — use the search/get tools directly.
```
**What this does.** The only door from chat to writes. The docstring is itself part of the contract — it is what the LLM reads when deciding to call the tool, including good/bad `task_description` examples ("Update chassis charge on load DRAY-12345 to 18 days at $40/day" vs "ok do it").

```python
# portpro-ai-agents/app/agents/comms/delegation_tool.py:77-99
    state = tool_context.state or {}
    agent_id = state.get("agent_id", "helen")
    carrier_id = state.get("carrier_id")
    user_id = state.get("user_id")
    auth_token = state.get("auth_token")
    session_id = state.get("session_id")
    shared_opik_tracer = state.get("_shared_opik_tracer")
    customer_id = state.get("customer_id") or ""

    if not carrier_id or not auth_token:
        return _make_result("error", "Missing required context (carrier_id or auth_token)", "", runner=agent_id)

    if not task_description or len(task_description.strip()) < 10:
        return _make_result("error", ...)
```
**What this does.** All execution identity comes from session state seeded by the runner — never from the LLM's arguments. The LLM only supplies the task text; carrier, user, token, and target pipeline are fixed by the turn's context. Sub-10-character tasks are rejected to force self-contained instructions.

```python
# portpro-ai-agents/app/agents/comms/delegation_tool.py:174-184
        elif agent_id == "overrides_manager":
            result = await _run_overrides_manager_pipeline(
                task_description=task_description,
                carrier_id=carrier_id,
                user_id=user_id,
                auth_token=auth_token,
                session_id=session_id,
                context_data=state.get("context_data", ""),
            )
```
**What this does.** Dispatch arm for override-review threads (one of nine `_run_*_pipeline` arms). Note it passes `state["context_data"]` — the original AI-log + thread blob — alongside the task. The manager acts on precise charge/pricing ids; Commander's paraphrased task text alone would not be safe to act on.

```python
# portpro-ai-agents/app/agents/comms/delegation_tool.py:514-531
    from app.agents.overrides_manager.runner import run_overrides_manager

    chunks = []
    try:
        async for chunk, is_final in run_overrides_manager(
            user_id=user_id,
            carrier_id=carrier_id,
            session_id=session_id or f"comms_delegate_{carrier_id}",
            auth_token=auth_token,
            message=task_description,
            context_data=context_data or "",
        ):
            if chunk:
                chunks.append(chunk)
    except Exception as e:
        return _make_result("error", str(e), "", runner="overrides_manager")

    raw_output = "".join(chunks)
```
**What this does.** Runs the standalone Overrides Manager runner end to end inside the tool call, collecting its streamed output. The manager gathers evidence (tariff, playbook, override history), decides, and emits a JSON decision block in its text.

```python
# portpro-ai-agents/app/agents/comms/delegation_tool.py:541-544
    # Stash the RAW agent text (with its ```json decision block) so the
    # pick→apply bridge can parse the decision after the DCA turn — the DCA
    # final message paraphrases and drops the block.
    _stash_overrides_raw_output(session_id, raw_output)
```
**What this does.** The handoff trick. Commander's final chat message is a paraphrase and loses the machine-readable decision, so the raw output is stashed in a session-keyed dict (`delegation_tool.py:25-39`). After the turn, the auto-response service calls `pop_overrides_raw_output(session_id)` to parse the verdict and drive the backend apply/reject bridge — the last hop of [[Override Review Integration]].
