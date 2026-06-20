---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Ship CXC Inc - Carter
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Ship CXC Inc - Carter (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**8% auto.** Of 98 charge lines, **8 from the tariff** (auto-approvable) and **90 hand-keyed**. *(observed)*

- Charge sets: 9 (PAID:7, DRAFT:1, INVOICED:1).
- Load types: IMPORT:9.
- Biggest manual charge: **Chassis (8)** — #1 conversion target.
- Approved by: Andrea:9 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $45 (8x), $50 (1x) | days the chassis is held | mixed (1t/8m) |
| pier_pass | flat pass-through — $78 (9x) | pass-through (port receipt) | mixed (1t/8m) |
| Base Price | flat per move — $450 (8x), $22.35 (1x) | the lane (load itself) | mixed (1t/8m) |
| CLEAN TRUCK FEE | confirm — $20 (9x) | confirm source | mixed (1t/8m) |
| PIER PASS ADMIN FEE | flat pass-through — $25 (8x), $50 (1x) | pass-through (port receipt) | mixed (1t/8m) |
| bonded_cargo_charge | flat — $195 (8x) | bonded move flag | manual |
| transload | flat — $650 (7x), $800 (1x) | transload job | manual |
| PrePull | flat — $125 (6x), $150 (1x) | pull date vs last-free-day | mixed (1t/6m) |
| YARD STORAGE | per day — $45 (6x), $50 (1x) | yard dwell days | mixed (1t/6m) |
| EMPTY LOAD STORAGE | per day — $45 (4x), $50 (1x) | empty dwell days | mixed (1t/4m) |
| other | unknown — $175 (5x) | unknown | manual |
| PORT DETENTION | per hour after free — $70 (5x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| DELIVERY DETENTION | per hour after free — $70 (4x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| PERMIT TO TRANSFER | confirm — $175 (3x) | confirm source | manual |
| flip charge | flat — $72.61 (1x) | flip move evidence | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **bonded_cargo_charge** — Bonded-cargo handling. *(inferred)*
- **transload** — Cross-dock/transload service. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **other** — UNDEFINED line — must be identified before trust. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **PERMIT TO TRANSFER** — (confirm meaning with carrier). *(inferred)*
- **flip charge** — Flip charge. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (8%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 9 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 9/9 sets · $45(8x), $50(1x) · 1t/8m

> Loads: JYCT_M107384, JYCT_M107555, JYCT_M107556, JYCT_M107803, JYCT_M107991, JYCT_M108221, JYCT_M108288, JYCT_M108798, JYCT_M108806

**pier_pass** — 9/9 sets · $78(9x) · 1t/8m

> Loads: JYCT_M107384, JYCT_M107555, JYCT_M107556, JYCT_M107803, JYCT_M107991, JYCT_M108221, JYCT_M108288, JYCT_M108798, JYCT_M108806

**Base Price** — 9/9 sets · $450(8x), $22.35(1x) · 1t/8m

> Loads: JYCT_M107384, JYCT_M107555, JYCT_M107556, JYCT_M107803, JYCT_M107991, JYCT_M108221, JYCT_M108288, JYCT_M108798, JYCT_M108806

**CLEAN TRUCK FEE** — 9/9 sets · $20(9x) · 1t/8m

> Loads: JYCT_M107384, JYCT_M107555, JYCT_M107556, JYCT_M107803, JYCT_M107991, JYCT_M108221, JYCT_M108288, JYCT_M108798, JYCT_M108806

**PIER PASS ADMIN FEE** — 9/9 sets · $25(8x), $50(1x) · 1t/8m

> Loads: JYCT_M107384, JYCT_M107555, JYCT_M107556, JYCT_M107803, JYCT_M107991, JYCT_M108221, JYCT_M108288, JYCT_M108798, JYCT_M108806

**bonded_cargo_charge** — 8/9 sets · $195(8x) · manual

> Loads: JYCT_M107384, JYCT_M107555, JYCT_M107556, JYCT_M107803, JYCT_M107991, JYCT_M108221, JYCT_M108288, JYCT_M108798

**transload** — 8/9 sets · $650(7x), $800(1x) · manual

> Loads: JYCT_M107384, JYCT_M107555, JYCT_M107556, JYCT_M107803, JYCT_M107991, JYCT_M108221, JYCT_M108288, JYCT_M108798

**PrePull** — 7/9 sets · $125(6x), $150(1x) · 1t/6m

> Loads: JYCT_M107384, JYCT_M107555, JYCT_M107556, JYCT_M107803, JYCT_M108221, JYCT_M108798, JYCT_M108806

**YARD STORAGE** — 7/9 sets · $45(6x), $50(1x) · 1t/6m

> Loads: JYCT_M107384, JYCT_M107555, JYCT_M107556, JYCT_M107803, JYCT_M108221, JYCT_M108798, JYCT_M108806

**EMPTY LOAD STORAGE** — 5/9 sets · $45(4x), $50(1x) · 1t/4m

> Loads: JYCT_M107384, JYCT_M107991, JYCT_M108221, JYCT_M108288, JYCT_M108806

**other** — 5/9 sets · $175(5x) · manual

> Loads: JYCT_M107384, JYCT_M107555, JYCT_M107556, JYCT_M107803, JYCT_M107991

**PORT DETENTION** — 4/9 sets · $70(5x) · manual

> Loads: JYCT_M107803, JYCT_M107991, JYCT_M108221, JYCT_M108798

**DELIVERY DETENTION** — 4/9 sets · $70(4x) · manual

> Loads: JYCT_M107555, JYCT_M107556, JYCT_M108288, JYCT_M108798

**PERMIT TO TRANSFER** — 3/9 sets · $175(3x) · manual

> Loads: JYCT_M108221, JYCT_M108288, JYCT_M108798

**flip charge** — 1/9 sets · $72.61(1x) · manual

> Loads: JYCT_M107991

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107384 | IMPORT | PAID | $1898 | 11 | Andrea | no |
| JYCT_M107555 | IMPORT | PAID | $1921.1 | 11 | Andrea | no |
| JYCT_M107556 | IMPORT | PAID | $1954 | 11 | Andrea | no |
| JYCT_M107803 | IMPORT | PAID | $1977.6 | 11 | Andrea | no |
| JYCT_M107991 | IMPORT | PAID | $1838.21 | 11 | Andrea | no |
| JYCT_M108221 | IMPORT | PAID | $2204 | 12 | Andrea | no |
| JYCT_M108288 | IMPORT | PAID | $1746.7 | 10 | Andrea | no |
| JYCT_M108798 | IMPORT | INVOICED | $2118 | 13 | Andrea | no |
| JYCT_M108806 | IMPORT | DRAFT | $848 | 8 | Andrea | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
