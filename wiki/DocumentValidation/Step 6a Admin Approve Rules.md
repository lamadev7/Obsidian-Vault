---
type: source-summary
tags: [document-validation, admin, approval, ai-rules]
sources: [[raw/DocumentValidation/Admin Rule Approval.md]], [[raw/DocumentValidation/codes/Admin Rule Approval Code.md]]
updated: 2026-06-02
---

# Step 6a — Admin Approve Rules

Wiki summary of [[raw/DocumentValidation/Admin Rule Approval.md|Admin Rule Approval]] (+ [[raw/DocumentValidation/codes/Admin Rule Approval Code.md|code walk]]). The **single approval gate** for an AI rule. On `/admin/ai-sops-playbook` an admin opens a playbook's **View Rules** modal and approves each [[AiRules|AiRule]], flipping `status` from `pending` → `approved`. The only approval action that matters for rule-firing — there is **no separate business/engineering step**. After this, the rule still won't fire until the SOP is activated in [[Step 7 Customer Portal Approval]].

## How it works
1. **Open the page** — `/admin/ai-sops-playbook` (admin role; server-side scope on every API). `routesConfig.jsx:2146`, `pages/admin/AISOPsPlaybook/index.js`.
2. **View Rules** — `onShowAIRules(row)` opens `AIRulesModal.jsx`, populated by `getAIRulesBySopId(sopId, 1000, 0)`.
3. **Approve a rule** — builds a 3-field payload `{ruleId, carrierId, status}`, POSTs, re-fetches to refresh the modal.
4. **Backend updates Mongo** — Joi-validated, admin-scoped, carrier-matched; sets `AiRules.status='approved'`. `portpro-backend/server/modules/admin/index.js:3104-3155`.
5. **Bulk path** — `handleToggleAllAIRules` fires one POST per rule concurrently via `Promise.all`.

## Endpoint
`POST /admin/update-ai-rule-status {ruleId, carrierId, status}` → Mongo `AiRules.status: pending → approved` (`admin-controller.js` `updateAiRuleStatus`). Distinct from the customer-side activation endpoint in [[Step 7 Customer Portal Approval]].

## Code highlights
- **FE handler** `useAISOPsPlaybook.js:778-808` — `setLoadingRuleId` guards double-submits; re-fetches after POST; touches **no** business/engineering flag.
- **Action creator** `AIRules/actionCreator.js:23-36` — thin JWT POST to `admin/update-ai-rule-status`.
- **Backend route** `admin/index.js:3104-3155` — three guards: **admin scope** (non-admin → 403), **Joi** (`status ∈ AI_RULE_STATUS` = approved/pending/rejected), **carrier match** in controller (no cross-carrier writes). Controller sets only the `status` field.

## Vestigial UI — Engineering / Business Approved
The DataGrid still renders **Engineering Approved** + **Business Approved** columns/buttons (`constants.js:198-205` / `216-221`). Their flags (`sop_embeddings.engineering_approved` / `business_approved`) are **not consumed by the live rule-firing gate** — only an unused draft-trial branch. Treat as legacy. Operative path: **View Rules → Approve**. See [[Runtime Validation Layers and Load Match]].

## Post-step state (single-approval)
| Field | State |
|-------|-------|
| AiRules.status | `approved` ✅ |
| AiRules.isActive | `true` ✅ |
| sop_embeddings.status | `pending` ❌ (activated at [[Step 7 Customer Portal Approval]]) |
| playbook_scenarios.is_active | `false` ❌ |

`status='approved'` is necessary but not sufficient — runtime also needs `isActive=true` + an active [[Playbook Scenarios|playbook_scenarios]] row. See [[Approval State Gate]].

## Failure cases
| Case | Behavior |
|------|----------|
| Double-click mid-request | `setLoadingRuleId` guards; second click no-op |
| Joi rejects payload | Toast "Failed to update rule status" |
| `carrierId` missing | Joi rejects (required) |
| Non-admin via direct API | 403 — scope check fails |
| Unapprove | Same endpoint, `status='pending'` — flips back (doesn't delete) |

## Related
[[User Manual Overview]] · [[Step 5 Flow E Document Validation]] (prev) · [[Step 7 Customer Portal Approval]] (next) · [[AiRules]] · [[Approval State Gate]] · [[Runtime Validation Layers and Load Match]]
