---
type: code-note
domain: DocumentValidation
source: portpro-frontend (CustomerSOPs) + portpro-ai-agents (ai_chat_v2)
updated: 2026-06-02
---

# Customer SOP Activation — code walk

> Explains: [[Customer SOP Activation]]

```jsx
// portpro-frontend/.../AIChatV2/Components/CustomerSOPs.jsx:788-803
const handleApproveClick = () => setShowApproveModal(true);
const handleApproveConfirm = () => {
  setShowApproveModal(false);
  if (activeTab === AI_CHAT_V2_ENUMS.PLAYBOOK) {
    setShowMigrateModal(true);        // Playbook: ask about migration first
  } else {
    handleReviewAction("approved", false);
  }
};
```
**Approve → migrate gate.** On the Playbook tab, confirming Approve opens a *second* modal asking whether to back-apply to existing in-flight loads. Other tabs activate directly.

```js
// portpro-frontend/.../CustomerSOPs.jsx:~680-784
const handleReviewAction = async (action, migrateExistingLoads = false) => {
  const freshData = await getCustomerSOPById(customer.id);          // avoid stale id
  const versionId = freshData?.playbook?.id;
  const activate = action === "approved";
  const payload = {
    sopEmbeddingId: versionId, action, comment: "",
    ...(activate ? { activate } : {}),
    ...(migrateExistingLoads ? { migrate_existing_loads: true } : {}),
  };
  await reviewApproval(payload);
  // refresh customer row → APPROVAL_STATUS.PUBLISHED
};
```
**The activation call.** `versionId` comes from a fresh fetch (not an optimistic patch) so it can't target a stale version. `activate` is only present for approvals; `migrate_existing_loads` comes from the migrate modal.

```js
// portpro-frontend/.../AIChatV2/actionCreators.js:1444-1476
export function reviewApproval(payload) {
  const url = `${config.aiAgentUrl}/api/ai-chat/v2/approval/${payload.sopEmbeddingId}/review`;
  return HTTP("post", null, {
    action: payload.action, comment: payload.comment ?? "",
    ...(Object.hasOwn(payload, "activate") ? { activate: payload.activate } : {}),
    ...(payload.migrate_existing_loads ? { migrate_existing_loads: true } : {}),
  }, { authorization: getStorage("token") }, url);
}
```
**Endpoint.** `POST /api/ai-chat/v2/approval/:sopEmbeddingId/review` — the carrier-published review, distinct from the admin `update-ai-rule-status` endpoint.

```sql
-- portpro-ai-agents ai_chat_v2.py /review handler → sop_vector.py:1441-1451
-- on action="approved" + activate=true:
-- 1) sop_embeddings.status='approved', is_active=true
-- 2) deactivate superseded scenario versions:
UPDATE playbook_scenarios
SET is_active = false
WHERE carrier_id = $1 AND is_active = true AND playbook_id != $2;
-- 3) if migrate_existing_loads: background task re-applies the rule to in-flight loads
```
**Backend side effects.** Activation + deactivation of older scenario versions happens **here**, not at rule-approval time — deliberately deferred (mirrors `_deactivate_old_scenarios`, `sop_vector.py:713`) so live scenarios aren't killed while a draft is being trialed. Migration is best-effort in the background.
