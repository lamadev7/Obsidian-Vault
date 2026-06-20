---
type: entity
tags: [document-validation, mongo, rules, data-model]
sources: [[raw/DocumentValidation/Admin Rule Approval.md]], [[raw/DocumentValidation/Runtime Rule Firing.md]], [[raw/DocumentValidation/codes/Admin Rule Approval Code.md]]
updated: 2026-06-02
---

# AiRules

Mongo `AiRules` collection — the executable rule that fires at runtime. Written during Flow E resumption (`create_document_rule`, [[Step 5 Flow E Document Validation|Step 5]]), approved at [[Step 6a Admin Approve Rules|Step 6]], read by [[Process Post AI Rules|processPostAIRules]] at [[Step 8 Runtime Rule Fire|Step 8]].

## Key fields
- `status` — `pending` | `approved` | `rejected` (`AI_RULE_STATUS`). The **single approval gate** flips this at Step 6.
- `isActive` / `isDeleted` — runtime filters.
- `carrierId`, customer match, `module` (e.g. `load`).
- `validation_script` / `execution_script` — the rule-executor runs validation → if true, executes (`publishException('DOC_REQUIREMENT')`).

## Lifecycle
- **Created** `pending` during Flow A resumption after Flow E.
- **Approved** at [[Step 6a Admin Approve Rules]] via `POST /admin/update-ai-rule-status {ruleId, carrierId, status}` → `status: pending → approved`. Three backend guards: admin scope, Joi (`status ∈ AI_RULE_STATUS`), carrier match. Bulk path = one POST per rule via `Promise.all`.
- **Fires** at runtime only when `status='approved' AND isActive AND !isDeleted AND customer match AND` an active [[Playbook Scenarios|playbook_scenarios]] row. See [[Approval State Gate]].

## Why AiRules AND load_doc_requirements both exist
`AiRules` (read by [[Process Post AI Rules|processPostAIRules]]) *raises the exception* at runtime. [[Load Doc Requirements|load_doc_requirements]] (read by the FE) shows the missing-doc chip *proactively*. Different audiences. Per [[Step 8 Runtime Rule Fire]].

## Related
[[Step 6a Admin Approve Rules]] · [[Step 8 Runtime Rule Fire]] · [[Process Post AI Rules]] · [[Approval State Gate]] · [[Playbook Scenarios]] · [[Load Doc Requirements]] · [[Runtime Validation Layers and Load Match]]
