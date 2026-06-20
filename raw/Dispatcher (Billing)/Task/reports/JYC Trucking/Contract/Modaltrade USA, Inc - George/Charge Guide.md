---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Modaltrade USA, Inc - George
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Modaltrade USA, Inc - George (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 19 charge lines, **0 from the tariff** (auto-approvable) and **19 hand-keyed**. *(observed)*

- Charge sets: 4 (PAID:3, INVOICED:1).
- Load types: EXPORT:4.
- Biggest manual charge: **Chassis (4)** — #1 conversion target.
- Approved by: Andrea:4 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $45 (4x) | days the chassis is held | manual |
| Base Price | flat per move — $450 (1x), $550 (1x) | the lane (load itself) | manual |
| PORT DETENTION | per hour after free — $70 (3x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| pier_pass | flat pass-through — $78 (1x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $20 (1x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $25 (1x) | pass-through (port receipt) | manual |
| PrePull | flat — $125 (1x) | pull date vs last-free-day | manual |
| EMPTY LOAD STORAGE | per day — $45 (1x) | empty dwell days | manual |
| DELIVERY DETENTION | per hour after free — $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| drop_charge | confirm — $200 (1x) | confirm source | manual |
| YARD STORAGE | per day — $45 (1x) | yard dwell days | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 4 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 4/4 sets · $45(4x) · manual

> Loads: JYCT_E107544, JYCT_E107899, JYCT_E107956, JYCT_E108608

**Base Price** — 4/4 sets · $450(1x), $550(1x), $430(1x) · manual

> Loads: JYCT_E107544, JYCT_E107899, JYCT_E107956, JYCT_E108608

**PORT DETENTION** — 2/4 sets · $70(3x) · manual

> Loads: JYCT_E107956, JYCT_E108608

**pier_pass** — 1/4 sets · $78(1x) · manual

> Loads: JYCT_E108608

**CLEAN TRUCK FEE** — 1/4 sets · $20(1x) · manual

> Loads: JYCT_E108608

**PIER PASS ADMIN FEE** — 1/4 sets · $25(1x) · manual

> Loads: JYCT_E108608

**PrePull** — 1/4 sets · $125(1x) · manual

> Loads: JYCT_E107956

**EMPTY LOAD STORAGE** — 1/4 sets · $45(1x) · manual

> Loads: JYCT_E107956

**DELIVERY DETENTION** — 1/4 sets · $70(1x) · manual

> Loads: JYCT_E107956

**drop_charge** — 1/4 sets · $200(1x) · manual

> Loads: JYCT_E107899

**YARD STORAGE** — 1/4 sets · $45(1x) · manual

> Loads: JYCT_E107544

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E107544 | EXPORT | PAID | $515 | 3 | Andrea | no |
| JYCT_E107899 | EXPORT | PAID | $720 | 3 | Andrea | no |
| JYCT_E107956 | EXPORT | PAID | $1044.7 | 6 | Andrea | no |
| JYCT_E108608 | EXPORT | INVOICED | $647.4 | 7 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
