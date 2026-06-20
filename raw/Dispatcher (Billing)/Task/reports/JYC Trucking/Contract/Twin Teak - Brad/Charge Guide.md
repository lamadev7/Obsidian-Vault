---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Twin Teak - Brad
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Twin Teak - Brad (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 17 charge lines, **0 from the tariff** (auto-approvable) and **17 hand-keyed**. *(observed)*

- Charge sets: 2 (INVOICED:1, PAID:1).
- Load types: IMPORT:2.
- Biggest manual charge: **pier_pass (2)** — #1 conversion target.
- Approved by: Andrea:2 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| pier_pass | flat pass-through — $78 (1x), $39 (1x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $20 (1x), $10 (1x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $25 (2x) | pass-through (port receipt) | manual |
| Base Price | flat per move — $575 (1x), $2100 (1x) | the lane (load itself) | manual |
| Chassis | per day — $45 (2x) | days the chassis is held | manual |
| PrePull | flat — $125 (2x) | pull date vs last-free-day | manual |
| PORT DETENTION | per hour after free — $80 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| YARD STORAGE | per day — $45 (1x) | yard dwell days | manual |
| DELIVERY DETENTION | per hour after free — $78 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| EMPTY LOAD STORAGE | per day — $45 (1x) | empty dwell days | manual |

## Definitions & deviations

- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*

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

**pier_pass** — 2/2 sets · $78(1x), $39(1x) · manual

> Loads: JYCT_M107843, JYCT_M108719

**CLEAN TRUCK FEE** — 2/2 sets · $20(1x), $10(1x) · manual

> Loads: JYCT_M107843, JYCT_M108719

**PIER PASS ADMIN FEE** — 2/2 sets · $25(2x) · manual

> Loads: JYCT_M107843, JYCT_M108719

**Base Price** — 2/2 sets · $575(1x), $2100(1x) · manual

> Loads: JYCT_M107843, JYCT_M108719

**Chassis** — 2/2 sets · $45(2x) · manual

> Loads: JYCT_M107843, JYCT_M108719

**PrePull** — 2/2 sets · $125(2x) · manual

> Loads: JYCT_M107843, JYCT_M108719

**PORT DETENTION** — 1/2 sets · $80(2x) · manual

> Loads: JYCT_M108719

**YARD STORAGE** — 1/2 sets · $45(1x) · manual

> Loads: JYCT_M108719

**DELIVERY DETENTION** — 1/2 sets · $78(1x) · manual

> Loads: JYCT_M108719

**EMPTY LOAD STORAGE** — 1/2 sets · $45(1x) · manual

> Loads: JYCT_M107843

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107843 | IMPORT | PAID | $2479 | 7 | Andrea | no |
| JYCT_M108719 | IMPORT | INVOICED | $1923.76 | 10 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
