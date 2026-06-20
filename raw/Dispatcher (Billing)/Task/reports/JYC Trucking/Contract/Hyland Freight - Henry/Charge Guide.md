---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Hyland Freight - Henry
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Hyland Freight - Henry (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 3 charge lines, **0 from the tariff** (auto-approvable) and **3 hand-keyed**. *(observed)*

- Charge sets: 1 (INVOICED:1).
- Load types: IMPORT:1.
- Biggest manual charge: **Base Price (1)** — #1 conversion target.
- Approved by: Andrea:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $475 (1x) | the lane (load itself) | manual |
| Chassis | per day — $45 (1x) | days the chassis is held | manual |
| SPLIT CHASSIS | per day — $75 (1x) | days the chassis is held | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **SPLIT CHASSIS** — Chassis usage. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 1 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Base Price** — 1/1 sets · $475(1x) · manual

> Loads: JYCT_M108767

**Chassis** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108767

**SPLIT CHASSIS** — 1/1 sets · $75(1x) · manual

> Loads: JYCT_M108767

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108767 | IMPORT | INVOICED | $595 | 3 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
