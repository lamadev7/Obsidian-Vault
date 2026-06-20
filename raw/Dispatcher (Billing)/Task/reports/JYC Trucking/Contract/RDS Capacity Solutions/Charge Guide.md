---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: RDS Capacity Solutions
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — RDS Capacity Solutions (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 64 charge lines, **0 from the tariff** (auto-approvable) and **64 hand-keyed**. *(observed)*

- Charge sets: 7 (PARTIALLY_PAID:5, PAID:2).
- Load types: IMPORT:7.
- Biggest manual charge: **PORT DETENTION (11)** — #1 conversion target.
- Approved by: Andrea:7 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| bonded_cargo_charge | flat — $180 (7x) | bonded move flag | manual |
| Base Price | flat per move — $375 (7x) | the lane (load itself) | manual |
| PrePull | flat — $125 (7x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $45 (7x) | yard dwell days | manual |
| Chassis | per day — $45 (7x) | days the chassis is held | manual |
| DELIVERY DETENTION | per hour after free — $70 (7x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| PORT DETENTION | per hour after free — $70 (11x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| reefer | confirm — $150 (6x) | confirm source | manual |
| EMPTY LOAD STORAGE | per day — $45 (5x) | empty dwell days | manual |

## Definitions & deviations

- **bonded_cargo_charge** — Bonded-cargo handling. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **reefer** — (confirm meaning with carrier). *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 7 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**bonded_cargo_charge** — 7/7 sets · $180(7x) · manual

> Loads: JYCT_M107874, JYCT_M107875, JYCT_M107876, JYCT_M107877, JYCT_M107878, JYCT_M107879, JYCT_M107880

**Base Price** — 7/7 sets · $375(7x) · manual

> Loads: JYCT_M107874, JYCT_M107875, JYCT_M107876, JYCT_M107877, JYCT_M107878, JYCT_M107879, JYCT_M107880

**PrePull** — 7/7 sets · $125(7x) · manual

> Loads: JYCT_M107874, JYCT_M107875, JYCT_M107876, JYCT_M107877, JYCT_M107878, JYCT_M107879, JYCT_M107880

**YARD STORAGE** — 7/7 sets · $45(7x) · manual

> Loads: JYCT_M107874, JYCT_M107875, JYCT_M107876, JYCT_M107877, JYCT_M107878, JYCT_M107879, JYCT_M107880

**Chassis** — 7/7 sets · $45(7x) · manual

> Loads: JYCT_M107874, JYCT_M107875, JYCT_M107876, JYCT_M107877, JYCT_M107878, JYCT_M107879, JYCT_M107880

**DELIVERY DETENTION** — 7/7 sets · $70(7x) · manual

> Loads: JYCT_M107874, JYCT_M107875, JYCT_M107876, JYCT_M107877, JYCT_M107878, JYCT_M107879, JYCT_M107880

**PORT DETENTION** — 7/7 sets · $70(11x) · manual

> Loads: JYCT_M107874, JYCT_M107875, JYCT_M107876, JYCT_M107877, JYCT_M107878, JYCT_M107879, JYCT_M107880

**reefer** — 6/7 sets · $150(6x) · manual

> Loads: JYCT_M107874, JYCT_M107875, JYCT_M107876, JYCT_M107877, JYCT_M107878, JYCT_M107879

**EMPTY LOAD STORAGE** — 5/7 sets · $45(5x) · manual

> Loads: JYCT_M107874, JYCT_M107875, JYCT_M107876, JYCT_M107877, JYCT_M107879

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107874 | IMPORT | PAID | $1811.8 | 10 | Andrea | no |
| JYCT_M107875 | IMPORT | PARTIALLY_PAID | $2005.7 | 10 | Andrea | no |
| JYCT_M107876 | IMPORT | PARTIALLY_PAID | $1682.8 | 10 | Andrea | no |
| JYCT_M107877 | IMPORT | PAID | $1818.8 | 9 | Andrea | no |
| JYCT_M107878 | IMPORT | PARTIALLY_PAID | $1445.1 | 8 | Andrea | no |
| JYCT_M107879 | IMPORT | PARTIALLY_PAID | $1906.3 | 10 | Andrea | no |
| JYCT_M107880 | IMPORT | PARTIALLY_PAID | $1264.7 | 7 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
