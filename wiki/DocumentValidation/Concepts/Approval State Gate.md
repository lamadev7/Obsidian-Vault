---
type: concept
tags: [document-validation, approval, gate, cheat-sheet]
sources: [[raw/DocumentValidation/Document Validation Overview.md]], [[raw/DocumentValidation/Admin Rule Approval.md]], [[raw/DocumentValidation/Customer SOP Activation.md]]
updated: 2026-06-02
---

# Approval State Gate

The cheat-sheet for "does this rule fire?" Per [[raw/DocumentValidation/Document Validation Overview.md|Document Validation Overview]]. Live model is a **single approval gate** (admin) + a **carrier activation** (customer-SOP review) — NOT the old three-button business/engineering workflow ([[Original Prompt Seed]]).

## The five conditions
[[Process Post AI Rules|processPostAIRules]] fires an [[AiRules|AiRule]] only when **all** hold:
```
AiRules.status        = 'approved'
AiRules.isActive      = true
AiRules.isDeleted     = false
customer              matches the load
playbook_scenarios.is_active = true
```

## State by stage
| Stage | AiRule.status | playbook_scenarios.is_active | sop_embeddings.status | Fires? |
|-------|---------------|------------------------------|------------------------|--------|
| After [[Step 5 Flow E Document Validation\|Step 5]] (save) | `pending` | `false` | `pending` | No |
| After [[Step 6a Admin Approve Rules\|Step 6]] (admin approves) | `approved` | `false` | `pending` | No |
| After [[Step 7 Customer Portal Approval\|Step 7]] (customer review) | `approved` | `true` | `approved` | **Yes** |

## The two not-the-gate fields
`sop_embeddings.business_approved` and `engineering_approved` exist as columns and still render buttons in the admin DataGrid, but are **read only by an unused draft-trial branch** — never by the live `processPostAIRules` path. See [[Runtime Validation Layers and Load Match]]. Do not reintroduce a "Step 6b/6c" model: it was removed because it isn't in the live code.

## document_validation exception
`document_validation` [[SOP Embeddings|SOPs]] skip this lifecycle entirely — **auto-approved + active on save** ([[Flow E Skill]]).

## Related
[[Step 6a Admin Approve Rules]] · [[Step 7 Customer Portal Approval]] · [[Process Post AI Rules]] · [[Runtime Validation Layers and Load Match]] · [[AiRules]] · [[Playbook Scenarios]] · [[SOP Embeddings]]
