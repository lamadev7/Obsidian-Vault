---
type: concept
tags: [document-validation, runtime, validation, load-match]
sources: [[raw/DocumentValidation/Document Validation Overview.md]], [[raw/DocumentValidation/Runtime Rule Firing.md]]
updated: 2026-06-02
---

# Runtime Validation Layers and Load Match

How an uploaded document is judged at runtime, and why the legacy `engineering_approved` / `business_approved` columns are NOT part of the rule-firing gate. Per [[raw/DocumentValidation/Document Validation Overview.md|Document Validation Overview]] (Step 8B) and [[raw/DocumentValidation/Runtime Rule Firing.md|Runtime Rule Firing]].

## The two validation layers
When a dispatcher drops a document into the [[Step 8 Runtime Rule Fire|runtime drop-zone]], `validateDocumentWithAI` runs the **doc-validator agent**, which applies two layers:

1. **Built-in load-match** — extracts physical identifiers (container, BOL, seal, booking) and checks they match the load (`is_load_match`). **`reference_number` is explicitly NOT matched** (it's a soft/mutable field).
2. **Customer SOP rules** — the `<doc_type>` rules authored in [[Step 5 Flow E Document Validation|Flow E]] (`v2RulesCompliance`), read from the `document_validation` [[SOP Embeddings|SOP]].

Verdict `VALIDATED` → no further `DOC_REQUIREMENT` exception. Rejections carry an `issueType`: **Load Match Issue** / clarity / classification fail / **rule failure**. Drives the [[Document Validation State]] machine.

## The firing gate (single-approval)
The live `processPostAIRules` path gates a rule ON only when:

```
AiRules.status = 'approved'
  AND AiRules.isActive = true
  AND AiRules.isDeleted = false
  AND customer matches
  AND playbook_scenarios.is_active = true
```

See [[Approval State Gate]] for the full cheat sheet. **The `engineering_approved` / `business_approved` columns on [[SOP Embeddings|sop_embeddings]] are NOT consulted here.** They exist as columns and the admin DataGrid still renders their buttons (`constants.js:198-205` / `216-221`), but they are read **only by an unused draft-trial branch** and are never set by the live app. Earlier drafts of the user manual described a separate Business/Engineering approval workflow ([[Original Prompt Seed]]); that workflow is not in the live code path. Approval = the single admin gate ([[Step 6a Admin Approve Rules]]) + carrier activation ([[Step 7 Customer Portal Approval]]).

## Worked example
Ross Logistics POD rule, driver arrives at the DELIVERLOAD stop:
1. `processPostAIRules` selects the approved+active POD [[AiRules|AiRule]] for the customer → `publishException('DOC_REQUIREMENT')`.
2. Dispatcher opens the load → red "Required: Proof of Delivery" drop-zone (from [[Load Doc Requirements|load_doc_requirements]]).
3. Dispatcher drops a POD PDF → `uploadDocumentForLoad` → `DocumentValidationState{PENDING}`.
4. Doc-validator: **Layer 1** confirms container/BOL match the load (`is_load_match`); **Layer 2** checks the `<proof_of_delivery>` SOP rules.
5. Pass both → `VALIDATED` → exception cleared, chip disappears. Fail Layer 1 → `REJECTED` with `issueType: Load Match Issue`.

## Related
[[Step 8 Runtime Rule Fire]] · [[Document Validation State]] · [[Approval State Gate]] · [[Process Post AI Rules]] · [[Step 5 Flow E Document Validation]] · [[SOP Embeddings]] · [[AiRules]]
