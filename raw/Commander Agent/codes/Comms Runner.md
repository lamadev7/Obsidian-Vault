---
type: code-note
domain: Commander Agent
source: portpro-ai-agents/app/agents/comms/runner.py
updated: 2026-06-10
---

# Comms Runner — code walk

> Explains: [[Commander Agent Overview]]

```python
# portpro-ai-agents/app/agents/comms/runner.py:478-489
async def run_drayage_comms_agent(
    user_id: str,
    carrier_id: str,
    session_id: str,
    auth_token: str,
    message: str,
    context_data: str = "",
    json_format: bool = False,
    opik_project_name: str = None,
    agent_id: str = "helen",
    **kwargs,
) -> AsyncGenerator[Tuple[str, bool], None]:
```
**What this does.** The single entry point for every chat turn (DM, thread, @mention). It is an async generator yielding `(text_chunk, is_final)` tuples that the messaging backend streams to the user. `context_data` is the full conversation blob built by the DM context / auto-response services; `agent_id` decides which domain personality and toolset this turn runs as.

```python
# portpro-ai-agents/app/agents/comms/runner.py:522-531
    captain_confidence: int = 5
    if agent_id == "captain":
        from app.agents.comms.classifier import resolve_captain_agent_id
        agent_id, captain_confidence = await resolve_captain_agent_id(
            message, context_data
        )
```
**What this does.** Captain mode: before anything is built (log prefix, MCP toolset, skill load all depend on `agent_id`), the classifier swaps `captain` for a concrete sub-agent id. The classifier only routes — entity resolution (which customer, which load) is the chosen sub-agent's job via its own tools. See [[codes/Captain Classifier]].

```python
# portpro-ai-agents/app/agents/comms/runner.py:541-543
    if agent_id == "sop_agent":
        from app.agents.comms.sop_bridge import handle_sop_turn
```
**What this does.** Playbook work bypasses the whole Commander runner. SOP V3 has its own persistent session (`DatabaseSessionService` + sidecar `active_sop_run_id`), so the bridge dispatches the turn there and yields a terminal marker immediately; the frontend's SopRunPanel subscribes to an SSE stream for live progress. Commander itself stays stateless per request.

```python
# portpro-ai-agents/app/agents/comms/runner.py:861-875
        from app.agents.agentic_ai.components.model_utils import get_gemini_retry_model
        # Gemini 3 requires temperature=1.0; lower values trigger documented
        # "infinite loops, degraded reasoning" (LiteLLM warning at runtime).
        selected_model = get_gemini_retry_model("gemini-3-flash-preview", temperature=1.0)

        agent = LlmAgent(
            name="DrayageCommsAgent",
            model=selected_model,
            instruction=instruction,
            tools=tools,
            output_key="comms_output",
            **opik_callbacks,
        )
```
**What this does.** Builds the actual ADK agent for this turn: model is `gemini-3-flash-preview` with temperature pinned at 1.0 (a real production incident — temp 0.2 made Gemini 3 loop through 30 LLM calls over 16 minutes before delegating). `instruction` comes from `instruction.py` (scope gate, capability inventory, schema reference); `tools` is the read-only-filtered MCP set plus `delegate_to_pipeline` and attachment tools.

```python
# portpro-ai-agents/app/agents/comms/runner.py:888-903
        initial_state = {
            "carrier_id": carrier_id,
            "auth_token": auth_token,
            "user_id": user_id,
            "session_id": session_id,
            "agent_id": agent_id,
            "conversation_id": conversation_id,
            "_shared_opik_tracer": shared_tracer,
            # Original conversation/AI-log context, so a delegated pipeline that needs
            # the EXACT source values (e.g. overrides_manager — it acts on precise
            # ids) can read it from state rather than DCA's paraphrased task.
            "context_data": context_data,
        }
        if sidecar_customer_id:
            initial_state["customer_id"] = sidecar_customer_id
```
**What this does.** Seeds the per-request session state that every tool reads. Two things matter most: `customer_id` comes from the DM sidecar so customer-scoped gates work on the first turn, and the raw `context_data` rides along so the overrides manager can act on exact ids instead of Commander's paraphrase — the contract [[Override Review Integration]] depends on.

```python
# portpro-ai-agents/app/agents/comms/runner.py:921-926
        session_service = InMemorySessionService()
        session = await session_service.create_session(
            app_name="drayage_comms",
            user_id=user_id or "system",
            state=initial_state,
        )
```
**What this does.** Commander has no cross-turn memory of its own — a fresh in-memory session per request. Continuity comes from the context blob (sidecar + rolling summary + up to 250 recent messages) injected each turn, not from the ADK session.
