---
type: entity
tags: [document-validation, postgres, sop, data-model]
sources: [[raw/DocumentValidation/Document Validation Overview.md]], [[raw/DocumentValidation/Flow E Document Validation.md]], [[raw/DocumentValidation/codes/Customer SOP Activation Code.md]]
updated: 2026-06-02
---

# SOP Embeddings

PostgreSQL `sop_embeddings` — the row that represents a customer SOP (playbook, contract, or `document_validation`). Created at [[User Manual Overview|Step 2]], activated at [[Step 7 Customer Portal Approval|Step 7]].

## Key columns
- `sop_type` — `"playbook"` | `"contract"` | `"document_validation"`.
- `customer_ids` — array; group SOPs hold multiple.
- `instructions` — the SOP body. For `document_validation`, tag-wrapped per doc type (`<proof_of_delivery>…</proof_of_delivery>`) — see [[Flow E Skill]].
- `status` — `"pending"` | `"approved"` | `"rejected"`.
- `is_active` — flipped `true` at carrier activation.
- `business_approved` / `engineering_approved` — **vestigial** columns; not consumed by the live rule-firing gate (only an unused draft-trial branch). See [[Runtime Validation Layers and Load Match]].

## Lifecycle
- **Step 2** — `POST /api/ai-chat/v2/sop` creates two rows per customer (PLAYBOOK + CONTRACT), both `instructions=""`, `status='pending'`. At most one playbook + one contract per customer (server-enforced).
- **Step 5 (Flow E)** — a third `document_validation` row is created (or one updated) via `save_sop_to_vector_db`; **auto-approved** (`status='approved'`, `is_active=true`) — skips the rule lifecycle. ONE per customer. Per [[Step 5 Flow E Document Validation]].
- **Step 7** — the playbook row flips `status='approved'`, `is_active=true` (`sop_vector.py:1441-1451`). Per [[Step 7 Customer Portal Approval]].

## Relations
- Compiled into [[Playbook Scenarios|playbook_scenarios]] (PG) — activated/deactivated alongside the SOP.
- The `document_validation` row is read by the runtime validator as **Layer 2** ([[Runtime Validation Layers and Load Match]]).
- Distinct from the runtime [[AiRules]] (Mongo) and [[Load Doc Requirements]] (Mongo), which the Flow-A `create_document_rule` step writes.

## Related
[[User Manual Overview]] · [[Step 5 Flow E Document Validation]] · [[Step 7 Customer Portal Approval]] · [[Playbook Scenarios]] · [[Approval State Gate]] · [[Flow E Skill]]
