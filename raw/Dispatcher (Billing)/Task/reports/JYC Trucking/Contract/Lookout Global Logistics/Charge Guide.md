---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Lookout Global Logistics
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Lookout Global Logistics (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**17% auto.** Of 95 charge lines, **16 from the tariff** (auto-approvable) and **79 hand-keyed**. *(observed)*

- Charge sets: 15 (PAID:9, INVOICED:4, DRAFT:2).
- Load types: IMPORT:15.
- Biggest manual charge: **Chassis (13)** — #1 conversion target.
- Approved by: Andrea:15 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $40 (13x), $50 (2x) | days the chassis is held | mixed (2t/13m) |
| PrePull | flat — $120 (13x), $150 (2x) | pull date vs last-free-day | mixed (2t/13m) |
| YARD STORAGE | per day — $40 (13x), $50 (2x) | yard dwell days | mixed (2t/13m) |
| Base Price | flat per move — $1650 (13x), $4.2 (2x) | the lane (load itself) | mixed (2t/13m) |
| scale_load | flat — $15 (7x), $15.25 (4x) | scale ticket | manual |
| EMPTY LOAD STORAGE | per day — $40 (8x), $50 (2x) | empty dwell days | mixed (2t/8m) |
| PORT DETENTION | per hour after free — $70 (7x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| pier_pass | flat pass-through — $78 (2x) | pass-through (port receipt) | tariff |
| CLEAN TRUCK FEE | confirm — $20 (2x) | confirm source | tariff |
| PIER PASS ADMIN FEE | flat pass-through — $50 (2x) | pass-through (port receipt) | tariff |
| WEEKEND DELIVERY | confirm — $100 (1x) | confirm source | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **WEEKEND DELIVERY** — (confirm meaning with carrier). *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (17%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 15 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 15/15 sets · $40(13x), $50(2x) · 2t/13m

> Loads: JYCT_M107370, JYCT_M107641, JYCT_M107727, JYCT_M107760, JYCT_M107792, JYCT_M107917, JYCT_M107965, JYCT_M107987, JYCT_M108287, JYCT_M108508, JYCT_M108585, JYCT_M108586, JYCT_M108723, JYCT_M108724, JYCT_M108753

**PrePull** — 15/15 sets · $120(13x), $150(2x) · 2t/13m

> Loads: JYCT_M107370, JYCT_M107641, JYCT_M107727, JYCT_M107760, JYCT_M107792, JYCT_M107917, JYCT_M107965, JYCT_M107987, JYCT_M108287, JYCT_M108508, JYCT_M108585, JYCT_M108586, JYCT_M108723, JYCT_M108724, JYCT_M108753

**YARD STORAGE** — 15/15 sets · $40(13x), $50(2x) · 2t/13m

> Loads: JYCT_M107370, JYCT_M107641, JYCT_M107727, JYCT_M107760, JYCT_M107792, JYCT_M107917, JYCT_M107965, JYCT_M107987, JYCT_M108287, JYCT_M108508, JYCT_M108585, JYCT_M108586, JYCT_M108723, JYCT_M108724, JYCT_M108753

**Base Price** — 15/15 sets · $1650(13x), $4.2(2x) · 2t/13m

> Loads: JYCT_M107370, JYCT_M107641, JYCT_M107727, JYCT_M107760, JYCT_M107792, JYCT_M107917, JYCT_M107965, JYCT_M107987, JYCT_M108287, JYCT_M108508, JYCT_M108585, JYCT_M108586, JYCT_M108723, JYCT_M108724, JYCT_M108753

**scale_load** — 11/15 sets · $15(7x), $15.25(4x) · manual

> Loads: JYCT_M107641, JYCT_M107727, JYCT_M107792, JYCT_M107917, JYCT_M107965, JYCT_M107987, JYCT_M108287, JYCT_M108508, JYCT_M108585, JYCT_M108586, JYCT_M108723

**EMPTY LOAD STORAGE** — 10/15 sets · $40(8x), $50(2x) · 2t/8m

> Loads: JYCT_M107641, JYCT_M107727, JYCT_M107965, JYCT_M107987, JYCT_M108287, JYCT_M108508, JYCT_M108585, JYCT_M108586, JYCT_M108724, JYCT_M108753

**PORT DETENTION** — 4/15 sets · $70(7x) · manual

> Loads: JYCT_M107965, JYCT_M108508, JYCT_M108585, JYCT_M108586

**pier_pass** — 2/15 sets · $78(2x) · tariff

> Loads: JYCT_M108724, JYCT_M108753

**CLEAN TRUCK FEE** — 2/15 sets · $20(2x) · tariff

> Loads: JYCT_M108724, JYCT_M108753

**PIER PASS ADMIN FEE** — 2/15 sets · $50(2x) · tariff

> Loads: JYCT_M108724, JYCT_M108753

**WEEKEND DELIVERY** — 1/15 sets · $100(1x) · manual

> Loads: JYCT_M108585

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107370 | IMPORT | PAID | $1930 | 4 | Andrea | no |
| JYCT_M107641 | IMPORT | PAID | $2145 | 6 | Andrea | no |
| JYCT_M107727 | IMPORT | PAID | $2145 | 6 | Andrea | no |
| JYCT_M107760 | IMPORT | PAID | $2130 | 4 | Andrea | no |
| JYCT_M107792 | IMPORT | PAID | $2065 | 5 | Andrea | no |
| JYCT_M107917 | IMPORT | PAID | $1865 | 5 | Andrea | no |
| JYCT_M107965 | IMPORT | PAID | $2037.6 | 7 | Andrea | no |
| JYCT_M107987 | IMPORT | PAID | $1985 | 6 | Andrea | no |
| JYCT_M108287 | IMPORT | PAID | $1985 | 6 | Andrea | no |
| JYCT_M108508 | IMPORT | INVOICED | $2185.25 | 8 | Andrea | no |
| JYCT_M108585 | IMPORT | INVOICED | $2125.25 | 9 | Andrea | no |
| JYCT_M108586 | IMPORT | INVOICED | $1985.25 | 8 | Andrea | no |
| JYCT_M108723 | IMPORT | INVOICED | $1985.25 | 5 | Andrea | no |
| JYCT_M108724 | IMPORT | DRAFT | $1698 | 8 | Andrea | yes |
| JYCT_M108753 | IMPORT | DRAFT | $1698 | 8 | Andrea | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
