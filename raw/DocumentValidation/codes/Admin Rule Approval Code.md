---
type: code-note
domain: DocumentValidation
source: portpro-frontend + portpro-backend (admin AI-SOPs)
updated: 2026-06-02
---

# Admin Rule Approval — code walk

> Explains: [[Admin Rule Approval]]

```js
// portpro-frontend/src/pages/admin/AISOPsPlaybook/useAISOPsPlaybook.js:778-808
const updateAIRuleStatus = useCallback(async (rule, status) => {
  setLoadingRuleId(rule?._id)
  const params = { ruleId: rule?._id, carrierId: rule?.carrierId, status } // AI_RULE_STATUS.APPROVED
  await updateAiRuleStatus(params)
  const sopId = getSopIdFromRow(selectedAIRulesRow)
  if (sopId) await fetchAIRules(sopId, true, { isGeneric, carrierId })
  toastr.show(`Rule ${status} successfully`, "success")
  // finally: clear loading + close confirmation modal
}, [selectedAIRulesRow, fetchAIRules, activeTab])
```
**FE handler.** The whole approval is a 3-field payload — `ruleId`, `carrierId`, `status`. `setLoadingRuleId` guards double-submits (the spinner makes a second click a no-op). After the POST it re-fetches the rule list so the row repaints to `approved` / `Unapprove`. No business/engineering flag is touched here.

```js
// portpro-frontend/src/pages/admin/AIRules/actionCreator.js:23-36
export function updateAiRuleStatus(params) {
  return new Promise((resolve, reject) => {
    HTTP('post', 'admin/update-ai-rule-status', params, { authorization: getStorage('token') })
      .then((result) => resolve(result.data.data))
      .catch(reject)
  })
}
```
**Action creator.** Thin POST wrapper carrying the JWT. The endpoint is `admin/update-ai-rule-status` — distinct from the customer-side activation endpoint used in [[Customer SOP Activation]].

```js
// portpro-backend/server/modules/admin/index.js:3104-3155
server.route({
  method: 'POST', path: '/admin/update-ai-rule-status',
  handler: async (request, h) => {
    const payloadData = { ...request.payload, userData: request.pre.getUserDetail }
    const result = await controllers.AdminController.updateAiRuleStatus(payloadData)
    return appUtilityFunctions.sendSuccess(null, result)
  },
  options: {
    auth: { strategy: 'JwtAuth', scope: ['admin'] },
    validate: { payload: Joi.object({
      ruleId: Joi.string().required(),
      status: Joi.string().required().valid(...Object.values(AI_RULE_STATUS)),
      carrierId: Joi.string().required(),
    }).options({ abortEarly: false, stripUnknown: true }) },
  },
})
```
**Backend route.** Three guards before the write: **admin scope** (non-admin → 403), **Joi** (`status` must be in `AI_RULE_STATUS` = approved/pending/rejected), and **carrier match** in the controller (no cross-carrier writes). The controller sets the Mongo `AiRules.status` field — nothing else.

```js
// portpro-frontend/.../useAISOPsPlaybook.js:810-830  (bulk toggle)
await Promise.all(
  ruleIds.map((ruleId) => {
    const rule = aiRules.find(r => r._id === ruleId)
    return updateAiRuleStatus({ ruleId, carrierId: rule?.carrierId, status })
  })
)
```
**Bulk path.** For playbooks with many pending rules — one POST per rule, fired concurrently. Same endpoint, same per-rule carrier scoping.
