---
type: source-summary
tags: [billing, jyc-trucking, findings, real-data]
updated: 2026-06-01
---

# JYC Trucking Billing Findings

First real-data look at [[Dispatcher Billing — Overview|dispatcher billing]] for a live carrier. Validates the concepts in [[Billing Flow]] and feeds the [[Charge Auto-Approval Task]].

- **Carrier:** `carlos@jyctrucking.com` (JYC Trucking).
- **Data window:** completed loads, delivery leg, 2025-06-01 → 2026-06-01.
- **Source:** dispatcher loads list API (`getDispatcherTMSLoads`), pulled via the app's own session.

## Headline finding — most loads are fully paid

Of ~1,146 completed delivery loads scanned, the [[Billing Flow|chargeStatus]] distribution:

| Status | Count | Meaning |
|---|---|---|
| **FULL_PAID** | 909 | customer paid in full — loop closed |
| BILLING | 178 | approved / invoiced, awaiting payment |
| UNAPPROVED | 32 | charges present, not yet approved |
| PARTIAL_PAID | 11 | partial payment received |
| REBILLING | 2 | invoice voided and re-issued |
| (blank) | 14 | no charges |

So the closed-loop "customer has paid the carrier" case is the **norm** here — ~80% of completed loads. Good substrate for validate-against-history.

## Data-access caveat (important)

The dispatcher **list** endpoint returns `chargeStatus` but **strips the billing detail** — `pricing`, `paymentHistory`, `totalAmount`, `paidAmount` all come back empty / zero in the list view (lazy-loaded elsewhere).

- ✅ Use `chargeStatus` to identify paid/approved loads from the list.
- ❌ Do **not** trust `paidAmount` / `isPaymentCompleted` from the list — they read as 0 even on FULL_PAID loads.
- To see real charge lines, approve timestamp, invoice date, payment date and lag → call the **per-load billing detail** endpoint (the call the app fires when opening a load's Billing tab). *Not yet captured.*

## Candidate pilot customer

**ARTIS SOLUTIONS LLC** appears ~10× among the first 50 FULL_PAID loads — high-volume, fully-paid, mostly import. Strong candidate for the first per-customer charge guide (lots of clean closed-loop history to validate against).

## Sample FULL_PAID load references

| Ref | Customer | Type | Completed |
|---|---|---|---|
| JYCT_M108697 | RT EXPRESS USA | import | 2026-05-20 |
| JYCT_E108713 | BEST BAY LOGISTICS | export | 2026-05-18 |
| JYCT_M108708 | ARTIS SOLUTIONS LLC | import | 2026-05-20 |
| JYCT_M108656 | LOGIFLEX CARGO | import | 2026-05-15 |

## Next step

Capture the per-load billing-detail call for one paid ref (e.g. `JYCT_M108697` or an ARTIS load) and walk it end-to-end on real numbers: every charge line, which were [[Tariffs and Missing Contracts|tariff-backed]], when approved, when invoiced, when paid — the lived version of [[Billing Flow]].
