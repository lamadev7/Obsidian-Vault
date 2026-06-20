---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Apostrophe Home
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Apostrophe Home (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 19 charge lines, **0 from the tariff** (auto-approvable) and **19 hand-keyed**. *(observed)*

- Charge sets: 2 (PAID:2).
- Load types: IMPORT:2.
- Biggest manual charge: **Chassis (2)** — #1 conversion target.
- Approved by: Andrea:2 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $50 (2x) | days the chassis is held | manual |
| YARD STORAGE | per day — $45 (2x) | yard dwell days | manual |
| PrePull | flat — $125 (2x) | pull date vs last-free-day | manual |
| pier_pass | flat pass-through — $78 (2x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $20 (2x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $25 (2x) | pass-through (port receipt) | manual |
| Base Price | flat per move — $720 (2x) | the lane (load itself) | manual |
| DELIVERY DETENTION | per hour after free — $70 (2x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| EMPTY LOAD STORAGE | per day — $45 (1x) | empty dwell days | manual |
| RESIDENTIAL SURCHARGE | confirm — $125 (1x) | confirm source | manual |
| PORT DETENTION | per hour after free — $70 (1x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **RESIDENTIAL SURCHARGE** — (confirm meaning with carrier). *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 2 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 2/2 sets · $50(2x) · manual

> Loads: JYCT_M107472, JYCT_M107479

**YARD STORAGE** — 2/2 sets · $45(2x) · manual

> Loads: JYCT_M107472, JYCT_M107479

**PrePull** — 2/2 sets · $125(2x) · manual

> Loads: JYCT_M107472, JYCT_M107479

**pier_pass** — 2/2 sets · $78(2x) · manual

> Loads: JYCT_M107472, JYCT_M107479

**CLEAN TRUCK FEE** — 2/2 sets · $20(2x) · manual

> Loads: JYCT_M107472, JYCT_M107479

**PIER PASS ADMIN FEE** — 2/2 sets · $25(2x) · manual

> Loads: JYCT_M107472, JYCT_M107479

**Base Price** — 2/2 sets · $720(2x) · manual

> Loads: JYCT_M107472, JYCT_M107479

**DELIVERY DETENTION** — 2/2 sets · $70(2x) · manual

> Loads: JYCT_M107472, JYCT_M107479

**EMPTY LOAD STORAGE** — 1/2 sets · $45(1x) · manual

> Loads: JYCT_M107472

**RESIDENTIAL SURCHARGE** — 1/2 sets · $125(1x) · manual

> Loads: JYCT_M107472

**PORT DETENTION** — 1/2 sets · $70(1x) · manual

> Loads: JYCT_M107472

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107472 | IMPORT | PAID | $1914.8 | 11 | Andrea | no |
| JYCT_M107479 | IMPORT | PAID | $1411 | 8 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
