---
type: concept
tags: [document-validation, flow-e, skill, agent]
sources: [[raw/DocumentValidation/Flow E Document Validation.md]], [[raw/DocumentValidation/codes/Flow E Document Validation Code.md]]
updated: 2026-06-02
---

# Flow E Skill

The `flow-e-document-validation` agent skill — the detour that captures ground-truth sample documents and writes a `document_validation` [[SOP Embeddings|SOP]]. Per [[raw/DocumentValidation/Flow E Document Validation.md|Flow E Document Validation]] (SKILL.md). Triggered when `save_playbook` detects the playbook names doc types (`playbook_tools.py:777-810`) → `document_validation_suggestion` + STOP_INSTRUCTION → orchestrator activates the skill **before** scenario processing, then resumes Flow A.

## Mechanics
- **Narrow tool surface** — only `search_entities`, `save_sop_to_vector_db`, `update_sop_in_vector_db`, `display_content`, `await_user_response`, `validate_uploaded_document_with_doc_agent`. **No** merge / playbook / scenario tools.
- **One SOP per customer** — search-first; never duplicate.
- **Tag per doc type** — `<proof_of_delivery>`, `<scale_ticket>`, `<all_type>` fallback. Overwritten in place **by tag** — the only SOP type with no merge tool (`re.sub` swaps the matching tag, preserves the rest).
- **Multi-document aware** — several types / several samples in one submit → one synthesized tag per type; preserves untouched tags on update.
- **Auto-approved** — `save_sop` returns `status="approved"` for `document_validation`; the validator uses it immediately. Skips the [[Approval State Gate|approval lifecycle]].
- **Refinement loop** — `validate_uploaded_document_with_doc_agent` for any "would this pass?"; surface the verdict + rationale verbatim; on disagreement update the SOP and re-validate. Stateless server-side, keyed by `(doc_sop_id, file)`.

## Critical rules (SKILL.md)
- No `upsert_sop_with_merge` (only SOP type without merge).
- No self-review, no scenario processing, no test loads, no approval workflow.
- ONE `document_validation` SOP per customer — search first, update if exists.
- ALWAYS wrap in tags (`<all_type>` fallback).
- **NEVER self-answer "is this valid?"** — route through the validator.

## Why session IDs stay on the playbook
For `document_validation`, `sop_vector_tools.py:193-200` deliberately does **not** overwrite `sop_id`/`sop_embedding_id` in session state, so the rules created later in Flow A (`create_document_rule`) link to the **playbook's** IDs, not this side SOP.

## Sample sourcing
Three paths, identical files payload: **upload**, **"show existing"** (the [[Document Picker Architecture|picker]] — see [[Flow E Existing Docs Picker Session]]), or **skip**.

## Related
[[Step 5 Flow E Document Validation]] · [[Document Picker Architecture]] · [[Flow E Existing Docs Picker Session]] · [[SOP Embeddings]] · [[Runtime Validation Layers and Load Match]] · [[Approval State Gate]]
