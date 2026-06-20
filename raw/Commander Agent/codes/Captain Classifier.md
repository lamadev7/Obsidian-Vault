---
type: code-note
domain: Commander Agent
source: portpro-ai-agents/app/agents/comms/classifier.py
updated: 2026-06-10
---

# Captain Classifier — code walk

> Explains: [[Captain Routing and Classifier]]

```python
# portpro-ai-agents/app/agents/comms/classifier.py:315-320
    if context_data and "playbook_patch_suggestion" in context_data.lower():
        logger.info(
            "[CaptainClassifier] Short-circuit: 'playbook_patch_suggestion' in "
            "context → routing to patch_agent (deterministic)"
        )
        return "patch_agent", 10
```
**What this does.** First deterministic gate. Patch cards are posted as captain with the system marker `playbook_patch_suggestion`; thread replies carry it in the context blob. Because the tag is system-emitted (not fuzzy user intent), a string check beats an LLM vote — and `patch_agent` is reachable *only* through this route. A miss would dump a bare "ok" on `helen`, a live mutating billing agent.

```python
# portpro-ai-agents/app/agents/comms/classifier.py:327-333
    if context_data and "override_review" in context_data.lower():
        logger.info(
            "[CaptainClassifier] Short-circuit: 'override_review' found in "
            "context → routing to overrides_manager"
        )
        # (agent_id, confidence) — the caller unpacks a 2-tuple; deterministic → max confidence.
        return "overrides_manager", 10
```
**What this does.** The override hook. Any reply inside an override-review card thread routes to the Overrides Manager regardless of message content. This single check is what makes the override feature's chat surface reliable — see [[Override Review Integration]].

```python
# portpro-ai-agents/app/agents/comms/classifier.py:345-356
        _call_kwargs = dict(
            model="gpt-4o-mini",
            messages=[
                {"role": "system", "content": _CLASSIFIER_SYSTEM},
                {"role": "user", "content": user_content},
            ],
            temperature=0,
            response_format={
                "type": "json_schema",
                "json_schema": {
                    "name": "captain_routing",
                    "strict": True,
```
**What this does.** The LLM path for everything else: a cheap `gpt-4o-mini` call at temperature 0 with a strict JSON schema forcing `{"agent_id", "confidence"}`. The user turn wraps context and message in `<context>`/`<message>` delimiters so untrusted chat content stays separated from the routing instructions.

```python
# portpro-ai-agents/app/agents/comms/classifier.py:269-286
    h = f"{message or ''} {context_data or ''}".lower()
    is_recon = "reconcile" in h or "reconciliation" in h
    is_invoice_csv = (
        ("chassis id" in h and ("charge type" in h or "hire" in h))
        or ("chassis" in h and "invoice" in h)
        or (".csv" in h and "invoice" in h)
    )
    if is_recon and is_invoice_csv:
        return "vendor_pay_agent", 1
    ...
    return _DEFAULT_AGENT_ID, 1
```
**What this does.** Degraded fallback, used only when the classifier model never produced a usable answer (timeout, 429, malformed JSON). It refuses to blindly default a known high-risk case — an attached invoice/chassis CSV reconciliation — onto billing; that goes to `vendor_pay_agent` so the recon skill at least runs. Confidence is pinned to 1 to mark the route as degraded in logs.
