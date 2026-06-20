---
type: code-note
domain: Override Flow
source: portpro-frontend/src/pages/tms/Load/Billing/*, .../OverrideReviewCard/*, portpro-backend/.../override-validator/index.js, portpro-ai-agents/.../override_review_actions.py
updated: 2026-06-20
---

# Billing Surface and Reject — code walk

> Explains: [[Override Flow v2]] · [[User Flow]] steps 3 & 9 · [[Technical Reference]] pillars 2 & 6.
> Covers how the TMS billing table reflects an override (chip / ghost row), the buttonless card, and
> the flag-gated Reject (FE idempotency + ai endpoint). Code verbatim from `origin/feat/override-v2`.

## Billing — optimistic chip / ghost

```js
// portpro-frontend/.../BillingCard/Charges/ChargesTable.js:1086-1101
                if (ovAdd.blocked) {
                    this.refreshAfterBlockedOverride();
                    this.props.onOverrideCaptured?.({
                        chargeId: payload.chargeId, pricing: payload.pricing,
                        chargeName: payload?.pricing?.chargeName || payload?.pricing?.name,
                        reason: ovAdd.reason, action: OVERRIDE_EVENTS.ADD_PRICING,
                    });
                }
```
**Record-only ADD → instant ghost.** Firing `onOverrideCaptured` shows the ghost row immediately; the
history refetch reconciles it. (`ovUpd.blocked` at ~L1275 does the same for UPDATE, passing
`pricingId` so the chip matches the exact row.)

```js
// portpro-frontend/src/pages/tms/Load/PricingComponent.js:137-147
    overriddenPrevValue(field) {
        const m = this.props.overriddenMark;
        if (!m || !Array.isArray(m.changedFields) || !m.changedFields.includes(field)) return null;
        const prev = m.previousPricing ? m.previousPricing[field] : undefined;
        if (prev === undefined || prev === null || prev === "") return "—";
        if (String(prev) === String(this.props.item?.[field] ?? "")) return null;
        return prev;
    }
```
**The "→ prev value" hint.** Returns the pre-override value for a changed field (em-dash for a
first-time set) → the yellow hint next to the overridden value.

```js
// portpro-frontend/src/pages/tms/Load/Billing/index.js:142-172
        if (row?.overrideType === 'ADD_PRICING') {
          if (row?.decision === 'rejected') return;                 // rejected ADD hides; log stays
          if (!grouped[row.chargeId]) grouped[row.chargeId] = [];
          grouped[row.chargeId].push(row);
          return;
        }
        if (row?.decision === 'observed' && row?.overrideType === 'UPDATE_PRICING') {
          const cs = (marks[row.chargeId] = marks[row.chargeId] || { byPricingId: {}, byChargeName: {} });
          const mark = {
            reason: row.reason, action: row.overrideType, at: row.createdAt, by: row?.actor?.name,
            changedFields: row.changedFields || null, previousPricing: row.previousPricing || null,
          };
          const pid = row?.pricing?._id;
          if (pid && !cs.byPricingId[pid]) cs.byPricingId[pid] = mark;
          const cn = String(row?.pricing?.chargeName || row?.chargeName || '').toUpperCase();
          if (cn && !cs.byChargeName[cn]) cs.byChargeName[cn] = mark;
        }
```
**The two render paths from one ledger.** ADD_PRICING → a ghost row (unless rejected, then hidden but
the log stays); observed UPDATE_PRICING → a "mark" (chip + prev hints) keyed by pricing `_id` with a
charge-name fallback. This is what makes the billing table and the Commander card show the same
override.

## The buttonless card

```jsx
// portpro-frontend/.../OverrideReviewCard/index.jsx:21-47
const OverrideReviewCard = ({ data, variant = "full", flat = false, messageId = null }) => {
  const minimal = variant === "minimal";
  const reviewStatus = (data && data.review_status) || "pending_review";
  return (
    <>
      <div className={flat ? "mb-1" : "card p-10 mb-1 bg-white rounded-5 shadow-sm"} style={{ maxWidth: 460 }}>
        <OverrideReviewCardBody data={data} variant={variant} reviewStatus={reviewStatus} ...>
          {!minimal && !TERMINAL_STATUSES.has(reviewStatus) ? (
            <div className="font-11 text-muted mt-2">Reply in this thread to ask the Overrides Manager about this override.</div>
          ) : null}
        </OverrideReviewCardBody>
        <OverrideRejectButton messageId={messageId} reviewStatus={reviewStatus} />
      </div>
```
**Observe mode = no Approve/Reject anywhere on the card** in any variant. The only control is the
flag-gated `OverrideRejectButton`.

```js
// portpro-frontend/.../OverrideReviewCard/OverrideReviewCardBody.jsx:40-54
export const REVIEW_STATUS_META = {
  pending_review: { label: "Override blocked — submitted for review", className: "text-warning-500" },
  in_discussion: { label: "Under AI review", className: "text-primary-500" },
  executing: { label: "Applying…", className: "text-primary-500" },
  approved: { label: "Applied — override approved", className: "text-success-500" },
  rejected: { label: "Rejected — AI decision kept", className: "text-muted" },
  kept_ai: { label: "AI decision kept", className: "text-muted" },
  escalated: { label: "Escalated to a teammate", className: "text-warning-500" },
  execution_failed: { label: "Apply failed — needs attention", className: "text-error-500" },
};
export const TERMINAL_STATUSES = new Set(["approved", "rejected", "kept_ai", "execution_failed"]);
```
**`observed` is intentionally NOT a key** — it falls back to the neutral `pending_review` meta and is
absent from `TERMINAL_STATUSES`, so observe cards stay non-terminal (reject + thread hint remain
available).

## Reject — FE idempotency

```jsx
// portpro-frontend/.../OverrideReviewCard/OverrideRejectButton.jsx:12-67
const _rejectedMessageIds = new Set();

const OverrideRejectButton = ({ messageId, reviewStatus, onRejected }) => {
  const alreadyRejected = reviewStatus === "rejected" || (messageId && _rejectedMessageIds.has(messageId));
  const [state, setState] = useState(alreadyRejected ? "rejected" : "idle");
  if (!isOverrideRejectEnabled() || !messageId) return null;               // flag gate
  if (TERMINAL_STATUSES.has(reviewStatus) && reviewStatus !== "rejected") return null;
  const done = state === "rejected" || _rejectedMessageIds.has(messageId);
  const onReject = async (e) => {
    if (e && e.stopPropagation) e.stopPropagation();
    if (done || state === "rejecting") return;
    setState("rejecting");
    try {
      await rejectOverrideReview(messageId);
      _rejectedMessageIds.add(messageId);
      setState("rejected");
      if (typeof onRejected === "function") onRejected();
    } catch (err) { setState("idle"); }
  };
  // ... renders Reject / Rejecting… / Rejected
};
```
**Module-level `_rejectedMessageIds` Set** keeps the button "Rejected" across re-mounts — a parent
re-render rebuilds the card from stale `review_status`, so component state alone isn't enough. The
button is invisible unless `isOverrideRejectEnabled()` (default OFF). `onRejected` triggers a list
refetch so the card leaves the Verify queue.

## Reject — ai endpoint

```python
# portpro-ai-agents/app/routes/messaging/override_review_actions.py:428-457
@router.post("/{message_id}/reject")
async def reject_override_review(request: Request, message_id: UUID, body: RejectBody = RejectBody()):
    user = getattr(request.state, "user", None)
    if not user: return create_error_response(401, "Authentication required")
    message, channel, err = await _load_review_card(message_id, user)
    if err: return err
    review_status = ((message.exception_data or {}).get("review_status")) or "pending_review"
    if review_status in ("approved", "executing", "rejected"):
        return create_error_response(409, f"Override review already {review_status}")
    message_service = MessageService()
    rejected = await message_service.reject_override_review(
        message_id, _user_id_from_user(user), _user_name_from_user(user), note=body.note)
    if not rejected: return create_error_response(409, "Override review was already decided")
    carrier_id = _carrier_id_from_user(user)
    await _call_backend_decision(str(carrier_id), message.exception_data or {}, str(message_id))
    await _emit_card_update(carrier_id, channel.id, message_id, "rejected")
    await sync_override_log_status(carrier_id, message.exception_data, "rejected")
    return {"success": True, "review_status": "rejected"}
```
**Idempotent backstop (409 on already-decided), atomic flip, payload kept for audit.** It clears the
pending stamp on the charge, emits a Firebase `review_update`, and mirrors `rejected` onto the
AiRequestLog so the billing ghost drops.

```python
# portpro-ai-agents/app/services/messaging/message_service.py:713-715
            WHERE id = $1 AND is_deleted = FALSE AND exception_type = 'override_review'
              AND COALESCE(exception_data->>'review_status','pending_review')
                  IN ('pending_review','in_discussion','execution_failed','observed')
```
**Human Reject wins even mid-verify** — it accepts the `in_discussion` state, so a reject during an
analysis run still lands.

## Supporting — get-override-history

```javascript
// portpro-backend/server/modules/override-validator/index.js:510-572 (abridged)
        const query = { carrier: userData.carrierId, agentName: 'OVERRIDES_MANAGER', isDeleted: { $ne: true } };
        if (referenceNumber) {
          query.$or = [{ loadReferenceNumber: referenceNumber }, { 'metadata.reference_number': referenceNumber }];
        }
        if (chargeId) query['metadata.entity.chargeId'] = chargeId;
        const rows = (logs || []).map((l) => { const m = l.metadata||{}; const e = m.entity||{}; return {
            overrideType: m.action, chargeId: e.chargeId,
            pricing: e.pricing || null, previousPricing: e.previousPricing || null,
            changedFields: e.changedFields || null, reason: m.reason, decision: m.review_status,
        };});
```
**History is served from `override-validator/index.js`, not a load-charge controller** — it reads the
shared AiRequestLogs store (`agentName:'OVERRIDES_MANAGER'`), carrier-scoped with
`isDeleted:{$ne:true}`. `previousPricing` / `changedFields` are computed pre-apply by
`enrichOverrideEntity` (same file) via one projected read of the pre-change pricing row — that data
drives the billing ghost / chip / hints above.

## Related
[[Override Flow v2]] · [[Technical Reference]] · [[codes/Backend Validator and Capture]] ·
[[codes/Patch Accept and SOP Routing]]
