---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: LEX LOGISTICS
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — LEX LOGISTICS (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 44 charge lines, **0 from the tariff** (auto-approvable) and **44 hand-keyed**. *(observed)*

- Charge sets: 6 (PAID:6).
- Load types: IMPORT:6.
- Biggest manual charge: **PORT DETENTION (7)** — #1 conversion target.
- Approved by: Andrea:6 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| reefer | confirm — $150 (6x) | confirm source | manual |
| Base Price | flat per move — $450 (6x) | the lane (load itself) | manual |
| Chassis | per day — $45 (6x) | days the chassis is held | manual |
| PORT DETENTION | per hour after free — $70 (7x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| PrePull | flat — $100 (3x), $125 (1x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $45 (4x) | yard dwell days | manual |
| EMPTY LOAD STORAGE | per day — $45 (4x) | empty dwell days | manual |
| DELIVERY DETENTION | per hour after free — $70 (4x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| HEAVY CONTAINER FEE | flat — $185 (2x) | overweight flag | manual |
| WEEKEND DELIVERY | confirm — $100 (1x) | confirm source | manual |

## Definitions & deviations

- **reefer** — (confirm meaning with carrier). *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **HEAVY CONTAINER FEE** — Overweight container surcharge. *(inferred)*
- **WEEKEND DELIVERY** — (confirm meaning with carrier). *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 6 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**reefer** — 6/6 sets · $150(6x) · manual

> Loads: JYCT_M107430, JYCT_M107431, JYCT_M107508, JYCT_M107648, JYCT_M107649, JYCT_M107788

**Base Price** — 6/6 sets · $450(6x) · manual

> Loads: JYCT_M107430, JYCT_M107431, JYCT_M107508, JYCT_M107648, JYCT_M107649, JYCT_M107788

**Chassis** — 6/6 sets · $45(6x) · manual

> Loads: JYCT_M107430, JYCT_M107431, JYCT_M107508, JYCT_M107648, JYCT_M107649, JYCT_M107788

**PORT DETENTION** — 5/6 sets · $70(7x) · manual

> Loads: JYCT_M107430, JYCT_M107431, JYCT_M107508, JYCT_M107648, JYCT_M107649

**PrePull** — 4/6 sets · $100(3x), $125(1x) · manual

> Loads: JYCT_M107430, JYCT_M107431, JYCT_M107648, JYCT_M107788

**YARD STORAGE** — 4/6 sets · $45(4x) · manual

> Loads: JYCT_M107430, JYCT_M107431, JYCT_M107648, JYCT_M107788

**EMPTY LOAD STORAGE** — 4/6 sets · $45(4x) · manual

> Loads: JYCT_M107430, JYCT_M107431, JYCT_M107648, JYCT_M107649

**DELIVERY DETENTION** — 4/6 sets · $70(4x) · manual

> Loads: JYCT_M107431, JYCT_M107508, JYCT_M107648, JYCT_M107649

**HEAVY CONTAINER FEE** — 2/6 sets · $185(2x) · manual

> Loads: JYCT_M107431, JYCT_M107648

**WEEKEND DELIVERY** — 1/6 sets · $100(1x) · manual

> Loads: JYCT_M107648

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107430 | IMPORT | PAID | $879.1 | 7 | Andrea | no |
| JYCT_M107431 | IMPORT | PAID | $1148.3 | 9 | Andrea | no |
| JYCT_M107508 | IMPORT | PAID | $839.6 | 5 | Andrea | no |
| JYCT_M107648 | IMPORT | PAID | $1597.7 | 11 | Andrea | no |
| JYCT_M107649 | IMPORT | PAID | $1291.1 | 7 | Andrea | no |
| JYCT_M107788 | IMPORT | PAID | $905 | 5 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
