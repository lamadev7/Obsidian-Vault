---
type: index
updated: 2026-06-02
---

# Wiki Index

Content catalog. One line per page. Updated on every ingest. Read first when answering a query.

## Document Validation

Authoring → approval → runtime pipeline for doc-validation rules in PortPro TMS. **Single-approval model** (admin gate + carrier activation; the old business/engineering steps are not in the live code path).

### Source summaries
- [[wiki/DocumentValidation/User Manual Overview]] — 8-step user flow, single-gate lifecycle, approval cheat-sheet
- [[wiki/DocumentValidation/Original Prompt Seed]] — informal seed prompt; where it diverged from the live single-gate model
- [[wiki/DocumentValidation/Step 5 Flow E Document Validation]] — Flow-E detour, tag-wrap, `save_sop_to_vector_db`, refinement loop
- [[wiki/DocumentValidation/Step 6a Admin Approve Rules]] — `/admin/ai-sops-playbook` View Rules, `updateAiRuleStatus`, single gate
- [[wiki/DocumentValidation/Step 7 Customer Portal Approval]] — `/review` activation, `migrate_existing_loads`, scenario activation
- [[wiki/DocumentValidation/Step 8 Runtime Rule Fire]] — `processPostAIRules` + drop-zone render
- [[wiki/DocumentValidation/Runtime Validation Layers and Load Match]] — built-in load-match + customer SOP rules; why business/engineering flags are NOT the gate
- [[wiki/DocumentValidation/Flow E Existing Docs Picker Session]] — 4-session build log: picker design, structural SSE side-channel, security remediation, group-SOP fairness

### Entities
- [[wiki/DocumentValidation/Entities/SOP Embeddings]] — PG `sop_embeddings`; playbook/contract/document_validation rows + status flags
- [[wiki/DocumentValidation/Entities/AiRules]] — Mongo `AiRules`; status gate, validation/execution scripts
- [[wiki/DocumentValidation/Entities/Playbook Scenarios]] — PG `playbook_scenarios`; compiled plans, activate/deactivate
- [[wiki/DocumentValidation/Entities/Load Doc Requirements]] — Mongo `load_doc_requirements`; FE drop-zone source
- [[wiki/DocumentValidation/Entities/Document Validation State]] — runtime state machine post-upload

### Concepts
- [[wiki/DocumentValidation/Concepts/Approval State Gate]] — 5-condition cheat sheet for "does the rule fire?"
- [[wiki/DocumentValidation/Concepts/Flow E Skill]] — `flow-e-document-validation` skill mechanics
- [[wiki/DocumentValidation/Concepts/Process Post AI Rules]] — runtime rule-firing chain
- [[wiki/DocumentValidation/Concepts/Document Picker Architecture]] — existing-docs picker, structural SSE side-channel, security model

## Raw sources (`raw/`)

Domain folder `raw/DocumentValidation/` — restructured 2026-06-02 to the vault-notes convention (clear filenames + `/codes`). Index note: [[raw/DocumentValidation/Document Validation Overview.md|Document Validation Overview]].

- `raw/DocumentValidation/Document Validation Overview.md` — the 8-step flow index → [[wiki/DocumentValidation/User Manual Overview]]
- `raw/DocumentValidation/Prompt.md` — original seed prompt → [[wiki/DocumentValidation/Original Prompt Seed]]
- `raw/DocumentValidation/Flow E Document Validation.md` (+ `codes/Flow E Document Validation Code.md`) → [[wiki/DocumentValidation/Step 5 Flow E Document Validation]]
- `raw/DocumentValidation/Admin Rule Approval.md` (+ `codes/Admin Rule Approval Code.md`) → [[wiki/DocumentValidation/Step 6a Admin Approve Rules]]
- `raw/DocumentValidation/Customer SOP Activation.md` (+ `codes/Customer SOP Activation Code.md`) → [[wiki/DocumentValidation/Step 7 Customer Portal Approval]]
- `raw/DocumentValidation/Runtime Rule Firing.md` (+ `codes/Runtime Rule Firing Code.md`) → [[wiki/DocumentValidation/Step 8 Runtime Rule Fire]] · [[wiki/DocumentValidation/Runtime Validation Layers and Load Match]]
- `raw/DocumentValidation/Existing Docs Picker — Build Log.md` — design + security + fairness session log → [[wiki/DocumentValidation/Flow E Existing Docs Picker Session]] · [[wiki/DocumentValidation/Concepts/Document Picker Architecture]]
