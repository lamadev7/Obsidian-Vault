---
type: entity
tags: [document-validation, postgres, scenarios, data-model]
sources: [[raw/DocumentValidation/Document Validation Overview.md]], [[raw/DocumentValidation/Customer SOP Activation.md]], [[raw/DocumentValidation/codes/Customer SOP Activation Code.md]]
updated: 2026-06-02
---

# Playbook Scenarios

PostgreSQL `playbook_scenarios` — the compiled, executable plans derived from a playbook [[SOP Embeddings|SOP]]. Compiled at [[Step 5 Flow E Document Validation|Step 5]] (`is_active=false`), activated at [[Step 7 Customer Portal Approval|Step 7]].

## Lifecycle
- **Compiled** during Flow A resumption: `compile_playbook_scenarios` → `review_compiled_plans` → `save_compiled_plans` → rows written with `is_active=false`. Per [[Step 5 Flow E Document Validation]].
- **Activated** at [[Step 7 Customer Portal Approval|Step 7]]: on `approved`+`activate`, the `/review` handler sets the SOP active **and deactivates superseded versions**:
  ```sql
  UPDATE playbook_scenarios
  SET is_active = false
  WHERE carrier_id = $1 AND is_active = true AND playbook_id != $2;
  ```
  Deferred to activation time (mirrors `_deactivate_old_scenarios`, `sop_vector.py:713`) **so live scenarios aren't killed while a draft is trialed**.

## Role in the firing gate
`playbook_scenarios.is_active=true` is one of the five conditions [[Process Post AI Rules|processPostAIRules]] requires before an [[AiRules|AiRule]] fires — see [[Approval State Gate]]. Until Step 7 it stays `false`, so an admin-approved rule still won't fire.

## Related
[[Step 5 Flow E Document Validation]] · [[Step 7 Customer Portal Approval]] · [[SOP Embeddings]] · [[Approval State Gate]] · [[AiRules]]
