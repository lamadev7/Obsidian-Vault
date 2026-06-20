---
type: code-note
domain: DocumentValidation
source: portpro-ai-agents/app/agents/billing_agent_v2/sop_vector_tools.py + flow-e skill
updated: 2026-06-02
---

# Flow E Document Validation — code walk

> Explains: [[Flow E Document Validation]]

```python
# sop_vector_tools.py:126-224  save_sop_to_vector_db
result = await service.save_sop(
    carrier_id=carrier_id,
    instructions=instructions,           # "<proof_of_delivery>…</proof_of_delivery>"
    sop_type=sop_type,                   # "document_validation"
    customer_ids=customer_ids,
    test_loads=test_loads,               # None for doc_validation
    auth_token=auth_token,
)
sop_id, sop_embedding_id = result.get("sop_id"), result.get("id")
status = result.get("status", "pending_approval")
```
**Save entry.** Stores the tag-wrapped `instructions` verbatim (no merge, no tag filtering — whatever tags you pass are what persist). `test_loads` is None: doc-validation SOPs don't use test loads.

```python
# sop_vector_tools.py:193-200  — session state NOT overwritten for doc_validation
if tool_context:
    if sop_type != "document_validation":
        tool_context.state["sop_id"] = sop_id
        tool_context.state["sop_embedding_id"] = sop_embedding_id
        tool_context.state["sop_type"] = sop_type
    tool_context.state["sop_instructions"] = instructions
```
**Why session IDs stay on the playbook.** For `document_validation` the `sop_id`/`sop_embedding_id` in session state are deliberately left pointing at the **playbook**, so the rules created later (back in Flow A) link to the playbook's IDs, not this side SOP.

```python
# sop_vector_tools.py:202-208  — auto-approved
if status == "approved":
    message = "SOP saved and is now active!"
else:
    message = "SOP saved. New additions need to go through the review process."
```
**Auto-approve.** `service.save_sop` returns `status="approved"` for `document_validation` — no human gate. The validator uses it immediately (it skips the rule approval lifecycle entirely).

```python
# update path (existing SOP) — merge by tag, preserve other tags
existing = search_result.instructions
if "<proof_of_delivery>" in existing:
    merged = re.sub(r"<proof_of_delivery>.*?</proof_of_delivery>",
                    new_pod_section, existing, flags=re.DOTALL)
else:
    merged = existing + "\n\n" + new_pod_section
await update_sop_in_vector_db(sop_id=existing_id, instructions=merged)
```
**Update = replace by tag.** Only the touched doc-type tag is replaced/added; other types' tags survive. This is how multi-type SOPs accrete safely — see [[Flow E Multi-Document Save]].

```python
# refinement loop — validate, never self-answer
validate_uploaded_document_with_doc_agent(
    doc_type="Proof of Delivery",
    filename=None,                 # defaults to most recent upload
    inline_sop_override=None,      # or a draft SOP body for a dry run
)
# → { validator_verdict, classified_doc_type, additional_checks, rationale }
```
**Refinement.** The skill must call this for any "is this valid?" question and surface the verdict + rationale verbatim. Stateless server-side — keyed only by `(doc_sop_id, file)`; on disagreement the agent updates the SOP and re-validates the same file.
