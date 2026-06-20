---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: JJ UNITED CARGO - PETER
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — JJ UNITED CARGO - PETER (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 7 charge lines, **0 from the tariff** (auto-approvable) and **7 hand-keyed**. *(observed)*

- Charge sets: 1 (PAID:1).
- Load types: EXPORT:1.
- Biggest manual charge: **Chassis (1)** — #1 conversion target.
- Approved by: Andrea:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $40 (1x) | days the chassis is held | manual |
| pier_pass | flat pass-through — $78 (1x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $20 (1x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $25 (1x) | pass-through (port receipt) | manual |
| Base Price | flat per move — $910 (1x) | the lane (load itself) | manual |
| PORT DETENTION | per hour after free — $70 (1x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| DELIVERY DETENTION | per hour after free — $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*

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

**Chassis** — 1/1 sets · $40(1x) · manual

> Loads: JYCT_E107501

**pier_pass** — 1/1 sets · $78(1x) · manual

> Loads: JYCT_E107501

**CLEAN TRUCK FEE** — 1/1 sets · $20(1x) · manual

> Loads: JYCT_E107501

**PIER PASS ADMIN FEE** — 1/1 sets · $25(1x) · manual

> Loads: JYCT_E107501

**Base Price** — 1/1 sets · $910(1x) · manual

> Loads: JYCT_E107501

**PORT DETENTION** — 1/1 sets · $70(1x) · manual

> Loads: JYCT_E107501

**DELIVERY DETENTION** — 1/1 sets · $70(1x) · manual

> Loads: JYCT_E107501

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E107501 | EXPORT | PAID | $1242.3 | 7 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
