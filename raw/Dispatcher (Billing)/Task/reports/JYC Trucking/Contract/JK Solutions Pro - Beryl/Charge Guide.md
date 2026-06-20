---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: JK Solutions Pro - Beryl
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — JK Solutions Pro - Beryl (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**29% auto.** Of 28 charge lines, **8 from the tariff** (auto-approvable) and **20 hand-keyed**. *(observed)*

- Charge sets: 4 (PAID:2, DRAFT:1, INVOICED:1).
- Load types: IMPORT:4.
- Biggest manual charge: **Chassis (3)** — #1 conversion target.
- Approved by: Andrea:4 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $50 (4x) | days the chassis is held | mixed (1t/3m) |
| PrePull | flat — $150 (4x) | pull date vs last-free-day | mixed (1t/3m) |
| YARD STORAGE | per day — $50 (3x), $45 (1x) | yard dwell days | mixed (1t/3m) |
| Base Price | flat per move — $1680 (2x), $6.15 (1x) | the lane (load itself) | mixed (1t/3m) |
| EMPTY LOAD STORAGE | per day — $50 (2x), $45 (1x) | empty dwell days | mixed (1t/2m) |
| scale_load | flat — $15.25 (1x), $15 (1x) | scale ticket | manual |
| pier_pass | flat pass-through — $78 (1x) | pass-through (port receipt) | tariff |
| CLEAN TRUCK FEE | confirm — $20 (1x) | confirm source | tariff |
| PIER PASS ADMIN FEE | flat pass-through — $50 (1x) | pass-through (port receipt) | tariff |
| PORT DETENTION | per hour after free — $80 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| DELIVERY DETENTION | per hour after free — $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (29%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 4 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 4/4 sets · $50(4x) · 1t/3m

> Loads: JYCT_M107635, JYCT_M108177, JYCT_M108711, JYCT_M108796

**PrePull** — 4/4 sets · $150(4x) · 1t/3m

> Loads: JYCT_M107635, JYCT_M108177, JYCT_M108711, JYCT_M108796

**YARD STORAGE** — 4/4 sets · $50(3x), $45(1x) · 1t/3m

> Loads: JYCT_M107635, JYCT_M108177, JYCT_M108711, JYCT_M108796

**Base Price** — 4/4 sets · $1680(2x), $6.15(1x), $950(1x) · 1t/3m

> Loads: JYCT_M107635, JYCT_M108177, JYCT_M108711, JYCT_M108796

**EMPTY LOAD STORAGE** — 3/4 sets · $50(2x), $45(1x) · 1t/2m

> Loads: JYCT_M107635, JYCT_M108711, JYCT_M108796

**scale_load** — 3/4 sets · $15.25(1x), $15(1x), $14.75(1x) · manual

> Loads: JYCT_M107635, JYCT_M108177, JYCT_M108711

**pier_pass** — 1/4 sets · $78(1x) · tariff

> Loads: JYCT_M108796

**CLEAN TRUCK FEE** — 1/4 sets · $20(1x) · tariff

> Loads: JYCT_M108796

**PIER PASS ADMIN FEE** — 1/4 sets · $50(1x) · tariff

> Loads: JYCT_M108796

**PORT DETENTION** — 1/4 sets · $80(2x) · manual

> Loads: JYCT_M108711

**DELIVERY DETENTION** — 1/4 sets · $70(1x) · manual

> Loads: JYCT_M107635

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107635 | IMPORT | PAID | $2114.75 | 7 | Andrea | no |
| JYCT_M108177 | IMPORT | PAID | $2245 | 5 | Andrea | no |
| JYCT_M108711 | IMPORT | INVOICED | $1305.25 | 8 | Andrea | no |
| JYCT_M108796 | IMPORT | DRAFT | $3948 | 8 | Andrea | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
