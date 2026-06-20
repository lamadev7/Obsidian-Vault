---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: White Oak Logistics - Justin
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — White Oak Logistics - Justin (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 64 charge lines, **0 from the tariff** (auto-approvable) and **64 hand-keyed**. *(observed)*

- Charge sets: 12 (PAID:11, INVOICED:1).
- Load types: EXPORT:8, IMPORT:4.
- Biggest manual charge: **Chassis (11)** — #1 conversion target.
- Approved by: Andrea:12 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $50 (11x) | days the chassis is held | manual |
| PrePull | flat — $125 (11x) | pull date vs last-free-day | manual |
| Base Price | flat per move — $850 (11x) | the lane (load itself) | manual |
| EMPTY LOAD STORAGE | per day — $45 (9x) | empty dwell days | manual |
| YARD STORAGE | per day — $45 (8x) | yard dwell days | manual |
| PORT DETENTION | per hour after free — $70 (4x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| DELIVERY DETENTION | per hour after free — $70 (3x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| pier_pass | flat pass-through — $78 (2x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $20 (2x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $25 (2x) | pass-through (port receipt) | manual |
| PORT DRY RUN | flat — $100 (1x) | trouble-call evidence | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **PORT DRY RUN** — Dry run / trouble call. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 12 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 11/12 sets · $50(11x) · manual

> Loads: JYCT_E107357, JYCT_E107358, JYCT_E107642, JYCT_E107798, JYCT_E108232, JYCT_E108366, JYCT_E108808, JYCT_M107365, JYCT_M108311, JYCT_M108312, JYCT_M108313

**PrePull** — 11/12 sets · $125(11x) · manual

> Loads: JYCT_E107357, JYCT_E107358, JYCT_E107642, JYCT_E107798, JYCT_E108232, JYCT_E108366, JYCT_E108808, JYCT_M107365, JYCT_M108311, JYCT_M108312, JYCT_M108313

**Base Price** — 11/12 sets · $850(11x) · manual

> Loads: JYCT_E107357, JYCT_E107358, JYCT_E107642, JYCT_E107798, JYCT_E108232, JYCT_E108366, JYCT_E108808, JYCT_M107365, JYCT_M108311, JYCT_M108312, JYCT_M108313

**EMPTY LOAD STORAGE** — 9/12 sets · $45(9x) · manual

> Loads: JYCT_E107357, JYCT_E107358, JYCT_E107642, JYCT_E107798, JYCT_E108232, JYCT_E108366, JYCT_E108808, JYCT_M108311, JYCT_M108312

**YARD STORAGE** — 8/12 sets · $45(8x) · manual

> Loads: JYCT_E107357, JYCT_E107358, JYCT_E108366, JYCT_E108808, JYCT_M107365, JYCT_M108311, JYCT_M108312, JYCT_M108313

**PORT DETENTION** — 3/12 sets · $70(4x) · manual

> Loads: JYCT_E107357, JYCT_E107358, JYCT_E108808

**DELIVERY DETENTION** — 3/12 sets · $70(3x) · manual

> Loads: JYCT_E107358, JYCT_E107798, JYCT_M107365

**pier_pass** — 2/12 sets · $78(2x) · manual

> Loads: JYCT_E107642, JYCT_E107798

**CLEAN TRUCK FEE** — 2/12 sets · $20(2x) · manual

> Loads: JYCT_E107642, JYCT_E107798

**PIER PASS ADMIN FEE** — 2/12 sets · $25(2x) · manual

> Loads: JYCT_E107642, JYCT_E107798

**PORT DRY RUN** — 1/12 sets · $100(1x) · manual

> Loads: JYCT_E107654

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E107357 | EXPORT | PAID | $1282.4 | 6 | Andrea | no |
| JYCT_E107358 | EXPORT | PAID | $1309 | 7 | Andrea | no |
| JYCT_E107642 | EXPORT | PAID | $1288 | 7 | Andrea | no |
| JYCT_E107654 | EXPORT | PAID | $100 | 1 | Andrea | no |
| JYCT_E107798 | EXPORT | PAID | $1323 | 8 | Andrea | no |
| JYCT_E108232 | EXPORT | PAID | $1165 | 4 | Andrea | no |
| JYCT_E108366 | EXPORT | PAID | $1260 | 5 | Andrea | no |
| JYCT_E108808 | EXPORT | INVOICED | $1215 | 7 | Andrea | no |
| JYCT_M107365 | IMPORT | PAID | $1191.6 | 5 | Andrea | no |
| JYCT_M108311 | IMPORT | PAID | $1260 | 5 | Andrea | no |
| JYCT_M108312 | IMPORT | PAID | $1260 | 5 | Andrea | no |
| JYCT_M108313 | IMPORT | PAID | $1070 | 4 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
