---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: GOLD COAST LOGISTICS - PRIORITY 1
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — GOLD COAST LOGISTICS - PRIORITY 1 (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**12% auto.** Of 48 charge lines, **6 from the tariff** (auto-approvable) and **42 hand-keyed**. *(observed)*

- Charge sets: 4 (PAID:2, DRAFT:1, INVOICED:1).
- Load types: IMPORT:4.
- Biggest manual charge: **Base Price (4)** — #1 conversion target.
- Approved by: Maria:4 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $50 (4x) | days the chassis is held | mixed (1t/3m) |
| YARD STORAGE | per day — $50 (4x) | yard dwell days | mixed (1t/3m) |
| pier_pass | flat pass-through — $78 (4x) | pass-through (port receipt) | mixed (1t/3m) |
| CLEAN TRUCK FEE | confirm — $20 (4x) | confirm source | mixed (1t/3m) |
| PIER PASS ADMIN FEE | flat pass-through — $25 (3x), $50 (1x) | pass-through (port receipt) | mixed (1t/3m) |
| Base Price | flat per move — $1 (3x), $990 (1x) | the lane (load itself) | manual |
| PrePull | flat — $150 (4x) | pull date vs last-free-day | manual |
| BONDED 7512 FORM | flat — $0 (2x), $1 (1x) | bonded move flag | manual |
| HIGH VALUE CARGO | confirm — $0 (2x), $200 (1x) | confirm source | manual |
| scale_load | flat — $15 (2x), $15.25 (1x) | scale ticket | manual |
| DELIVERY DETENTION | per hour after free — $70 (3x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| bonded_cargo_charge | flat — $250 (2x), $1 (1x) | bonded move flag | manual |
| PORT DETENTION | per hour after free — $70 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| EMPTY LOAD STORAGE | per day — $50 (1x) | empty dwell days | tariff |
| other | unknown — $100 (1x), $200 (1x) | unknown | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **BONDED 7512 FORM** — Bonded-cargo handling. *(inferred)*
- **HIGH VALUE CARGO** — (confirm meaning with carrier). *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **bonded_cargo_charge** — Bonded-cargo handling. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **other** — UNDEFINED line — must be identified before trust. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (12%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 4 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 4/4 sets · $50(4x) · 1t/3m

> Loads: JYCT_M108034, JYCT_M108140, JYCT_M108556, JYCT_M108744

**YARD STORAGE** — 4/4 sets · $50(4x) · 1t/3m

> Loads: JYCT_M108034, JYCT_M108140, JYCT_M108556, JYCT_M108744

**pier_pass** — 4/4 sets · $78(4x) · 1t/3m

> Loads: JYCT_M108034, JYCT_M108140, JYCT_M108556, JYCT_M108744

**CLEAN TRUCK FEE** — 4/4 sets · $20(4x) · 1t/3m

> Loads: JYCT_M108034, JYCT_M108140, JYCT_M108556, JYCT_M108744

**PIER PASS ADMIN FEE** — 4/4 sets · $25(3x), $50(1x) · 1t/3m

> Loads: JYCT_M108034, JYCT_M108140, JYCT_M108556, JYCT_M108744

**Base Price** — 4/4 sets · $1(3x), $990(1x) · manual

> Loads: JYCT_M108034, JYCT_M108140, JYCT_M108556, JYCT_M108744

**PrePull** — 4/4 sets · $150(4x) · manual

> Loads: JYCT_M108034, JYCT_M108140, JYCT_M108556, JYCT_M108744

**BONDED 7512 FORM** — 3/4 sets · $0(2x), $1(1x) · manual

> Loads: JYCT_M108140, JYCT_M108556, JYCT_M108744

**HIGH VALUE CARGO** — 3/4 sets · $0(2x), $200(1x) · manual

> Loads: JYCT_M108140, JYCT_M108556, JYCT_M108744

**scale_load** — 3/4 sets · $15(2x), $15.25(1x) · manual

> Loads: JYCT_M108034, JYCT_M108140, JYCT_M108556

**DELIVERY DETENTION** — 3/4 sets · $70(3x) · manual

> Loads: JYCT_M108034, JYCT_M108140, JYCT_M108556

**bonded_cargo_charge** — 3/4 sets · $250(2x), $1(1x) · manual

> Loads: JYCT_M108034, JYCT_M108140, JYCT_M108556

**PORT DETENTION** — 2/4 sets · $70(2x) · manual

> Loads: JYCT_M108140, JYCT_M108556

**EMPTY LOAD STORAGE** — 1/4 sets · $50(1x) · tariff

> Loads: JYCT_M108744

**other** — 1/4 sets · $100(1x), $200(1x) · manual

> Loads: JYCT_M108034

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108034 | IMPORT | PAID | $2103 | 12 | Maria | no |
| JYCT_M108140 | IMPORT | PAID | $2202.7 | 13 | Maria | no |
| JYCT_M108556 | IMPORT | INVOICED | $2185.75 | 13 | Maria | no |
| JYCT_M108744 | IMPORT | DRAFT | $1738 | 10 | Maria | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
