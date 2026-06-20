---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Cranston Trucking - Andee
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Cranston Trucking - Andee (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 21 charge lines, **0 from the tariff** (auto-approvable) and **21 hand-keyed**. *(observed)*

- Charge sets: 3 (PAID:3).
- Load types: IMPORT:3.
- Biggest manual charge: **Chassis (3)** — #1 conversion target.
- Approved by: Andrea:3 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $45 (3x) | days the chassis is held | manual |
| YARD STORAGE | per day — $45 (3x) | yard dwell days | manual |
| pier_pass | flat pass-through — $78 (3x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $20 (3x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $25 (3x) | pass-through (port receipt) | manual |
| Base Price | flat per move — $595 (3x) | the lane (load itself) | manual |
| DELIVERY DETENTION | per hour after free — $70 (2x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| PORT DETENTION | per hour after free — $70 (1x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 3 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 3/3 sets · $45(3x) · manual

> Loads: JYCT_M107369, JYCT_M107655, JYCT_M107949

**YARD STORAGE** — 3/3 sets · $45(3x) · manual

> Loads: JYCT_M107369, JYCT_M107655, JYCT_M107949

**pier_pass** — 3/3 sets · $78(3x) · manual

> Loads: JYCT_M107369, JYCT_M107655, JYCT_M107949

**CLEAN TRUCK FEE** — 3/3 sets · $20(3x) · manual

> Loads: JYCT_M107369, JYCT_M107655, JYCT_M107949

**PIER PASS ADMIN FEE** — 3/3 sets · $25(3x) · manual

> Loads: JYCT_M107369, JYCT_M107655, JYCT_M107949

**Base Price** — 3/3 sets · $595(3x) · manual

> Loads: JYCT_M107369, JYCT_M107655, JYCT_M107949

**DELIVERY DETENTION** — 2/3 sets · $70(2x) · manual

> Loads: JYCT_M107369, JYCT_M107655

**PORT DETENTION** — 1/3 sets · $70(1x) · manual

> Loads: JYCT_M107655

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107369 | IMPORT | PAID | $988.6 | 7 | Andrea | no |
| JYCT_M107655 | IMPORT | PAID | $927.3 | 8 | Andrea | no |
| JYCT_M107949 | IMPORT | PAID | $763 | 6 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
