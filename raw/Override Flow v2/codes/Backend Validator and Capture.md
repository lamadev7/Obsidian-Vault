---
type: code-note
domain: Override Flow
source: portpro-frontend/src/utils/overrideValidator.js, portpro-backend/server/modules/override-validator/index.js
updated: 2026-06-20
---

# Backend Validator and Capture — code walk

> Explains: [[Override Flow v2]] · [[User Flow]] steps 1–3 · [[Technical Reference]] pillars 1–2.
> Covers the FE entry point, the block→observe apply, record-only ADD, and the fire-and-forget
> capture to ai-agents. All code verbatim from `origin/feat/override-v2` (line numbers approximate).

## FE entry point

```js
// portpro-frontend/src/utils/overrideValidator.js:64-71
export const tryOverrideValidator = async ({ event, payload, customerId, overrideDetails }) => {
  if (!isOverrideEnabledForCustomer(customerId)) {
    return { handled: false };
  }
  const referenceNumber =
    payload?.reference_number || payload?.referenceNumber || payload?.loadRef || null;
  const details = overrideDetails || buildFallbackDetails(payload);
```
**The single FE entry for every override.** If the customer is not override-enabled it returns
`{handled:false}` and the caller proceeds normally. Otherwise everything below runs.

```js
// portpro-frontend/src/utils/overrideValidator.js:72-89
  const outcome = await promptOverrideReason({
    validateReason: (candidateReason) =>
      validateOverrideReason({ event, reason: candidateReason, referenceNumber, overrideDetails: details }),
    submitReason: (finalReason) =>
      callOverrideValidator({ event, payload, customerId, reason: finalReason }),
  });
  if (!outcome) {
    return { handled: true, cancelled: true };
  }
  if (outcome.error) {
    toastr.show(extractValidatorError(outcome.error), "error");
    return { handled: true, error: outcome.error };
  }
  toastr.show(outcome.result?.message || "Override applied — logged for AI review", "info");
  return { handled: true, blocked: true, result: outcome.result, reason: outcome.reason };
```
**Both phases run inside one modal:** the LLM reason gate (`validateReason`, fail-open) then the
capture call (`submitReason`). **Observe semantics:** every outcome returns `handled:true` so the
caller never double-applies. `blocked:true` is kept as the legacy key callers refetch on — under
observe it just means "the validator owned this action." In observe mode the `error` branch is a
*real* validation failure (credit hold, terminal scope, charge-code), because the validator runs
the actual controllers.

```js
// portpro-frontend/src/services/overrideValidator.services.js:69-89
export const callOverrideValidator = ({ event, payload, customerId, reason }) => {
  const url = "override-validator";
  return new Promise((resolve, reject) => {
    HTTP("post", url, { event, payload, customerId, reason }, { authorization: getStorage("token") })
      .then((result) => {
        if (result?.status === 200) resolve(result?.data?.data);
        else reject(result?.data || new Error("Override validation failed"));
      })
      .catch((error) => reject(error));
  });
};
```
**POSTs to the backend `override-validator` endpoint** and resolves with the applied entity.

## BE — apply (block → observe)

```javascript
// portpro-backend/server/modules/override-validator/index.js:160-185
    const isRecordOnly = event === 'ADD_PRICING';
    const result = isRecordOnly
      ? null
      : await applyOverrideAction(userData, event, payload, overrideReason);
    const applied = !isRecordOnly;
```
**The heart of block→observe.** The override is applied through `applyOverrideAction` — the *same*
controller path the normal endpoint uses, so all controller validations run exactly once. The lone
exception is `ADD_PRICING`: `isRecordOnly` skips the apply so a brand-new charge is **never**
committed to the bill (it surfaces as a ghost row instead — see [[codes/Billing Surface and Reject]]).

```javascript
// portpro-backend/server/modules/override-validator/index.js:297-298
    // 7. Return the applied response — the controller result is the real applied entity.
    return { success: true, applied, blocked: !applied, event, result: result ?? null };
```
**Return shape.** `applied` is true for everything except record-only ADD; `blocked:!applied` keeps
the legacy key the FE refetches on.

## BE — capture (fire-and-forget) + audit

```javascript
// portpro-backend/server/modules/override-validator/index.js:177-211
    (async () => {
      let snapshot = null;
      try {
        snapshot = await buildOverrideSnapshot?.(userData.carrierId, referenceNumber, event, entity);
      } catch (snapErr) {
        logger?.warn?.(`[override-validator] snapshot build failed: ${snapErr?.message || snapErr}`);
      }
      try {
        await postOverrideReviewToNeedsReview({
          carrier_id: userData.carrierId,
          override_kind: DOCUMENT_EVENTS.has(event) ? 'document' : 'charge',
          action: event,
          reason: overrideReason,
          actor: { user_id: userData.userId?.toString?.() || userData._id?.toString?.(), name: userData.name },
          reference_number: referenceNumber,
          load_id: payload.loadId?.toString?.() || null,
          customer_id: customerId?.toString?.() || null,
          entity: { ...cardEntity, requestedAction: event, blocked: !applied, applied },
          snapshot,
          occurred_at: new Date().toISOString(),
        });
      } catch (cardErr) {
        logger?.warn?.(`[override-validator] record-card post failed: ${cardErr?.message || cardErr}`);
      }
    })();
```
**Capture is best-effort and decoupled from apply.** It runs in a detached async IIFE so a slow or
failing ai-agents call never blocks the biller. The snapshot and the card-post each fail soft (warn
only). The entity carries `blocked:!applied, applied` so the downstream surfaces know ADD → ghost vs
applied → observed. **Decision rationale:** capture fires on apply *success only* — a failed override
means nothing happened, so there is nothing to learn.

```javascript
// portpro-backend/server/modules/override-validator/index.js:50-66
const OVERRIDE_AUDIT_TYPE = {
  ADD_PRICING: 'CHARGE_ADD_OVERRIDE_APPLIED',
  UPDATE_PRICING: 'CHARGE_UPDATE_OVERRIDE_APPLIED',
  REMOVE_PRICING: 'CHARGE_REMOVE_OVERRIDE_APPLIED',
  APPROVE_CHARGE: 'CHARGE_APPROVE_OVERRIDE_APPLIED',
  UNAPPROVE_CHARGE: 'CHARGE_UNAPPROVE_OVERRIDE_APPLIED',
  VALIDATE_DOCUMENT: 'DOCUMENT_VALIDATE_OVERRIDE_APPLIED',
  INVALIDATE_DOCUMENT: 'DOCUMENT_INVALIDATE_OVERRIDE_APPLIED',
  REMOVE_DOCUMENT: 'DOCUMENT_REMOVE_OVERRIDE_APPLIED',
};
const OVERRIDE_AUDIT_TYPE_RECORD_ONLY = { ADD_PRICING: 'CHARGE_ADD_OVERRIDE_BLOCKED' };
```
**Audit taxonomy.** Applied events write `*_OVERRIDE_APPLIED`; the one record-only path (ADD) keeps
the historical `CHARGE_ADD_OVERRIDE_BLOCKED` type so existing consumers don't break.

## Related
[[Override Flow v2]] · [[Technical Reference]] · [[codes/Overrides Manager and Mining]] ·
[[codes/Billing Surface and Reject]]
