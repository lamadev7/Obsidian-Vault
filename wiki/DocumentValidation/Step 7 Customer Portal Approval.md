---
type: source-summary
tags: [document-validation, activation, approval, customer-portal]
sources: [[raw/DocumentValidation/Customer SOP Activation.md]], [[raw/DocumentValidation/codes/Customer SOP Activation Code.md]]
updated: 2026-06-02
---

# Step 7 — Customer Portal Approval

Wiki summary of [[raw/DocumentValidation/Customer SOP Activation.md|Customer SOP Activation]] (+ [[raw/DocumentValidation/codes/Customer SOP Activation Code.md|code walk]]). The carrier-side sign-off that makes an approved rule **live**. After admin approves the rules ([[Step 6a Admin Approve Rules]]), a dispatcher / ops manager opens the customer's SOP panel and clicks **Approve** — marking the [[SOP Embeddings|SOP]] `status='approved'` + `is_active=true`, deactivating older [[Playbook Scenarios|scenario]] versions, and (optionally) back-applying the rule to in-flight loads. **This is the moment the rule starts firing.**

## Why a separate carrier-side gate?
Admin approval certifies the rule's quality (PortPro side). This step is the carrier turning it **on** for their own loads — separating "PortPro approved the rule" from "the carrier wants it enforced." The carrier can **Approve** (activates carrier-wide), **Reject** (stays dormant; admin republishes), or **Approve + migrate** (also apply to existing in-flight loads via the `MigrateModal`).

## How it works
1. **Open the panel** — `/tms/ai-agent-hub?section=customer-sops` → customer → Playbook tab. Right panel shows `Rules: Approved` / `Status: Pending review` / `[Reject] [Approve]`. `CustomerSOPs.jsx:788-803`.
2. **Approve → (Playbook only) migrate modal** — confirming Approve on the Playbook tab opens a second modal: "apply to existing in-flight loads too?"
3. **`handleReviewAction`** — re-fetches the latest `versionId` (avoids stale IDs), builds the review payload (`activate:true`, optional `migrate_existing_loads`), POSTs.
4. **Endpoint** — `POST {aiAgentUrl}/api/ai-chat/v2/approval/:sopEmbeddingId/review` (distinct from the admin rule-approval endpoint in [[Step 6a Admin Approve Rules]]).
5. **Backend side effects** — mark SOP `status='approved'`+`is_active=true`; **deactivate old scenario versions** (deferred to here so live scenarios aren't killed during draft testing); if `migrate_existing_loads`, background task re-applies to in-flight loads. `ai_chat_v2.py` `/review` · `sop_vector.py:1441-1451`.

## Code highlights
- **Approve → migrate gate** `CustomerSOPs.jsx:788-803` — Playbook tab opens a second migrate modal; other tabs activate directly.
- **Activation call** `CustomerSOPs.jsx:~680-784` — `versionId` from a fresh `getCustomerSOPById` fetch (not optimistic) so it can't target a stale version; `activate` present only for approvals.
- **Endpoint wrapper** `AIChatV2/actionCreators.js:1444-1476`.
- **Backend SQL** `sop_vector.py:1441-1451` — on `approved`+`activate`: (1) `sop_embeddings.status='approved', is_active=true`; (2) `UPDATE playbook_scenarios SET is_active=false WHERE carrier_id=$1 AND is_active=true AND playbook_id!=$2` (deactivate superseded versions — mirrors `_deactivate_old_scenarios` at `sop_vector.py:713`); (3) optional background migration.

## Post-step state — rule LIVE
| Field | State |
|-------|-------|
| AiRules.status | `approved` ✅ |
| AiRules.isActive | `true` ✅ |
| sop_embeddings.status | `approved` ✅ |
| sop_embeddings.is_active | `true` ✅ |
| playbook_scenarios.is_active | `true` ✅ |

(No `business_approved`/`engineering_approved` in the gate — see [[Runtime Validation Layers and Load Match]].) Runtime firing: [[Step 8 Runtime Rule Fire]].

## Failure cases
| Case | Behavior |
|------|----------|
| Reject | `action='rejected'`, no `activate`. Dormant; admin republishes. |
| Approve without migrate | Fires on new events only; in-flight loads unaffected. |
| Migration partial failure | Background task logs per-load failures (Control Tower); successes keep the exception. |
| Group customer | Approval + migration apply to ALL customers in the group. |
| document_validation tab | `migrate_existing_loads` irrelevant — doc-validation SOPs auto-approve at save ([[Step 5 Flow E Document Validation]]); they skip this gate. |

## Related
[[User Manual Overview]] · [[Step 6a Admin Approve Rules]] (prev) · [[Step 8 Runtime Rule Fire]] (next) · [[SOP Embeddings]] · [[Playbook Scenarios]] · [[Approval State Gate]] · [[Runtime Validation Layers and Load Match]]
