---
type: source-summary
tags: [document-validation, flow-e, sop, vector-db]
sources: [[raw/DocumentValidation/Flow E Document Validation.md]], [[raw/DocumentValidation/codes/Flow E Document Validation Code.md]]
updated: 2026-06-02
---

# Step 5 — Flow E Document Validation

Wiki summary of [[raw/DocumentValidation/Flow E Document Validation.md|Flow E Document Validation]] (+ code walk [[raw/DocumentValidation/codes/Flow E Document Validation Code.md|Flow E Document Validation Code]]). When a playbook's prose names a document type (e.g. "require a valid POD"), `save_playbook` returns a `document_validation_suggestion` + STOP_INSTRUCTION and the orchestrator detours into the [[Flow E Skill|flow-e-document-validation skill]] **before** scenario processing. Flow E captures ground-truth sample documents and writes a `document_validation` [[SOP Embeddings|SOP]] — one tag per doc type — that the runtime validator later judges uploads against. Trigger: `playbook_tools.py:777-810` inside `save_playbook`.

## How it works
1. **Source the samples** — three replies: **upload** fresh files, **"show existing"** (the picker — past completed+billed-load docs, see [[Flow E Existing Docs Picker Session]] / [[Document Picker Architecture]]), or **skip**. All paths deliver an identical files payload.
2. **Analyze every uploaded document** — multimodal pass per file, grouped by distinct doc type. Multi-type/multi-sample aware: writes one synthesized tag per type; preserves existing tags for types not uploaded this turn.
3. **Report + confirm** — lists every detected type, then `await_user_response`.
4. **Search first** — `search_entities(sop_type="document_validation")`; only **ONE** `document_validation` SOP per customer.
5. **Tag-wrap + save/update** — wrap each type's rules in its `<doc_type>` tag; one `save_sop_to_vector_db` (new) or one merge-by-tag `update_sop_in_vector_db` (existing).
6. **Display** — `display_content(is_updated=True)` → `✅ Saved`.
7. **Refinement loop (optional)** — `validate_uploaded_document_with_doc_agent` on a test doc; surface the verdict verbatim; on disagreement update the SOP and re-validate. **Never self-answer "is this valid?"**

## What persists
```
sop_embeddings (PG):
  sop_type     : "document_validation"
  customer_ids : ["<customerId>"]
  instructions : "<proof_of_delivery>…</proof_of_delivery>"
  is_active    : true
  status       : "approved"   # auto-approved + active on save — skips the rule lifecycle
```
No [[AiRules]], no [[Playbook Scenarios|playbook_scenarios]], no [[Load Doc Requirements|load_doc_requirements]] — those come from the Flow A resumption that follows (`create_document_rule`). This SOP is read by the runtime validator as **Layer 2** — see [[Runtime Validation Layers and Load Match]].

## Code highlights
Per [[raw/DocumentValidation/codes/Flow E Document Validation Code.md|the code walk]]:
- **Save entry** `sop_vector_tools.py:126-224` — stores tag-wrapped `instructions` verbatim (no merge, no tag filtering); `test_loads=None`.
- **Session IDs stay on the playbook** (`:193-200`) — for `document_validation`, `sop_id`/`sop_embedding_id` in session state are deliberately left pointing at the playbook so the later Flow-A rules link to the playbook's IDs, not this side SOP.
- **Auto-approve** (`:202-208`) — `service.save_sop` returns `status="approved"` for `document_validation`; validator uses it immediately.
- **Update = replace by tag** — `re.sub` swaps only the touched `<doc_type>` section; other tags survive. How multi-type SOPs accrete safely.
- **Refinement** — `validate_uploaded_document_with_doc_agent` is stateless server-side, keyed by `(doc_sop_id, file)`; returns `{validator_verdict, classified_doc_type, additional_checks, rationale}`.

## Critical skill rules
No `upsert_sop_with_merge` (only SOP type without merge); no self-review, no scenario processing, no test loads, no approval workflow; ONE SOP per customer; ALWAYS wrap in tags (`<all_type>` fallback); never self-answer validity. Full mechanics: [[Flow E Skill]].

## Next
Flow A resumes → `create_document_rule` (writes [[Load Doc Requirements|load_doc_requirements]] + a `pending` [[AiRules|AiRule]]) → compile → save scenarios. Then [[Step 6a Admin Approve Rules]].

## Related
[[User Manual Overview]] · [[Flow E Skill]] · [[Document Picker Architecture]] · [[Flow E Existing Docs Picker Session]] · [[Runtime Validation Layers and Load Match]] · [[Step 6a Admin Approve Rules]] (next)
