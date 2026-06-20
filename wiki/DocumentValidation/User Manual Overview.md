---
type: overview
tags: [document-validation, drayage, ai-sops, approval-lifecycle]
sources: [[raw/DocumentValidation/Document Validation Overview.md]], [[raw/DocumentValidation/Prompt.md]]
updated: 2026-06-02
---

# User Manual Overview

The end-to-end flow that turns a dispatcher's plain sentence — `"require POD on driver arrived in deliver event"` — into a live rule that fires on the next matching load and validates the uploaded document at runtime. Per [[raw/DocumentValidation/Document Validation Overview.md|Document Validation Overview]]. Seeded by [[Original Prompt Seed]].

> **Single-approval model (2026-06-02).** Approval in the live code is **one gate**: an [[AiRules|AiRule]] is either `pending` or `approved` (Step 6, admin), and the SOP is activated once on the customer-SOP review screen (Step 7). Earlier drafts described separate "Business Approved" + "Engineering Approved" buttons; those flags (`sop_embeddings.business_approved` / `engineering_approved`) still exist as columns but are **not consumed by the live rule-firing path** — only by an unused draft-trial branch. See [[Runtime Validation Layers and Load Match]] and the [[Approval State Gate]].

## The 8 steps

| Step | Persona | Where | Outcome | Deep dive |
|------|---------|-------|---------|-----------|
| 1 | Dispatcher | `/tms/ai-agent-hub?section=customer-sops` | Customer list visible | — |
| 2 | Dispatcher | + New SOP modal | Empty playbook + contract rows | [[SOP Embeddings]] |
| 3 | Dispatcher | Chat panel (SSE) | Playbook draft + 1 `pending` AiRule + doc-requirement profile | — |
| 4 | Dispatcher | Chat panel | Test loads pinned to playbook | — |
| 5 | Dispatcher | Chat panel (Flow E) | `document_validation` SOP with ground-truth samples | [[Step 5 Flow E Document Validation]] |
| 6 | Admin | `/admin/ai-sops-playbook` → View Rules | AiRule `status='approved'` | [[Step 6a Admin Approve Rules]] |
| 7 | Dispatcher | Customer SOPs review | SOP `status='approved'`, scenarios live | [[Step 7 Customer Portal Approval]] |
| 8 | Runtime | Load detail | Required-doc chip; upload → validated | [[Step 8 Runtime Rule Fire]] |

## Step-by-step

1. **Open Customer SOPs** — Sidebar → AI Agent Hub → Customer SOPs (`/tms/ai-agent-hub?section=customer-sops`). URL-driven (`routesConfig.jsx:2374` → `AIAgentHubV3`; shell reads `?section=` at `AIAgentHubV3/index.jsx:89-95`, mounts `<CustomerSOPs />` at `:725`). Deep links + back/forward work; no hidden per-component state.
2. **Create the SOP** — `+ Create SOP` → pick customer → `Save`. `POST {aiAgentUrl}/api/ai-chat/v2/sop` creates two [[SOP Embeddings|sop_embeddings]] rows (PLAYBOOK + CONTRACT), both `instructions=""`. At most one playbook + one contract per customer (server-enforced). Code: `CustomerSOPModal.jsx`; `AIChatV2/actionCreators.js:49`; `ai_chat_v2.py:1796`.
3. **Send the rule in chat** — e.g. `Make POD required on driver arrived on deliver events`. `POST {aiAgentUrl}/api/exception-recommendation-agent/billing-sop-chat` (SSE). Skill `flow-a-full-pipeline`: `upsert_sop_with_merge` → `get_playbook_context_for_decision` → `verify_playbook_entities` → `review_playbook_text` → `display_content` → `await_user_response`. SSE event types: `text`, `todo`, `sop`. Code: `exception_recommendation_agent.py:256`; `sop/runner_v3.py`.
4. **Pick test loads** — agent asks for 2–3 load refs (`suggest_test_loads_for_customer`); `validate_test_loads` joins `loads × customers`, applies three filters: **exists** in carrier scope, **owned** by customer/group, **not COMPLETED**. Rejections: `COMPLETED`, `NOT_FOUND_OR_WRONG_CUSTOMER`, `NOT_FOUND_OR_OUTSIDE_GROUP`. Code: `playbook_tools.py` `validate_test_loads`.
5. **Provide sample documents (Flow E)** — `save_playbook` detects the playbook names doc types → returns `document_validation_suggestion` + STOP_INSTRUCTION → detours into the [[Flow E Skill|flow-e-document-validation skill]]. Three options: **upload** fresh, **"show existing"** ([[Document Picker Architecture|the picker]]), or **skip**. Persists a `document_validation` [[SOP Embeddings|SOP]] (auto-approved). Then Flow A resumes → `create_document_rule` writes a [[Load Doc Requirements|load_doc_requirements]] profile + a `pending` [[AiRules|AiRule]] → compile [[Playbook Scenarios|playbook_scenarios]] (`is_active=false`). Full: [[Step 5 Flow E Document Validation]].
6. **Admin approves the rules** — `/admin/ai-sops-playbook` → filter carrier → playbook row → **View Rules** → **Approve**. `POST /admin/update-ai-rule-status {ruleId, status:'approved', carrierId}` → `AiRules.status: pending → approved`. The **single approval gate**. Full: [[Step 6a Admin Approve Rules]].
7. **Activate on customer-SOP review** — Customer SOPs → customer → Playbook → **Review & Approve**. `POST {aiAgentUrl}/api/ai-chat/v2/approval/:sopEmbeddingId/review {action:'approved', activate:true, migrate_existing_loads?}`. Marks SOP `status='approved'`, activates its [[Playbook Scenarios|playbook_scenarios]] (deactivating prior versions). **The moment the rule goes live.** Full: [[Step 7 Customer Portal Approval]].
8. **Runtime fires + document validated** — driver taps "Arrived" → `PATCH tms/editTMSLoad` → [[Process Post AI Rules|processPostAIRules]] raises a `DOC_REQUIREMENT` exception; `NewDocumentsTab` renders a red drop-zone; upload runs [[Runtime Validation Layers and Load Match|two-layer validation]]. Full: [[Step 8 Runtime Rule Fire]].

## Approval-state cheat sheet (live path)

| Stage | AiRule.status | playbook_scenarios.is_active | sop_embeddings.status | Rule fires? |
|-------|---------------|------------------------------|------------------------|-------------|
| After Step 5 (save) | `pending` | `false` | `pending` | No |
| After Step 6 (admin approves) | `approved` | `false` | `pending` | No |
| After Step 7 (customer review) | `approved` | `true` | `approved` | **Yes** |

`processPostAIRules` fires only when `status='approved'` AND `isActive=true` AND `isDeleted=false` AND customer matches AND `playbook_scenarios.is_active=true`. `document_validation` SOPs skip this lifecycle (auto-active on save). Detail: [[Approval State Gate]].

## Related
[[Original Prompt Seed]] · [[Step 5 Flow E Document Validation]] · [[Step 6a Admin Approve Rules]] · [[Step 7 Customer Portal Approval]] · [[Step 8 Runtime Rule Fire]] · [[Runtime Validation Layers and Load Match]] · [[Flow E Existing Docs Picker Session]] · all [[index]] entities/concepts
