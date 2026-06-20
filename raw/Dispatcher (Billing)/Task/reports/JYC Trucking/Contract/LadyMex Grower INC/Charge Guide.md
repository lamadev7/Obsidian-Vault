---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: LadyMex Grower INC
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — LadyMex Grower INC (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**14% auto.** Of 28 charge lines, **4 from the tariff** (auto-approvable) and **24 hand-keyed**. *(observed)*

- Charge sets: 3 (PAID:2, DRAFT:1).
- Load types: IMPORT:3.
- Biggest manual charge: **HEAVY CONTAINER FEE (3)** — #1 conversion target.
- Approved by: Juan Andres:3 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| HEAVY CONTAINER FEE | flat — $250 (3x) | overweight flag | manual |
| reefer | confirm — $150 (3x) | confirm source | manual |
| Base Price | flat per move — $535 (2x), $480 (1x) | the lane (load itself) | manual |
| YARD STORAGE | per day — $45 (3x) | yard dwell days | manual |
| PrePull | flat — $125 (3x) | pull date vs last-free-day | manual |
| Chassis | per day — $45 (2x) | days the chassis is held | manual |
| EMPTY LOAD STORAGE | per day — $50 (1x), $45 (1x) | empty dwell days | mixed (1t/1m) |
| tri_axle | confirm — $90 (2x) | confirm source | manual |
| DELIVERY DETENTION | per hour after free — $70 (2x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| pier_pass | flat pass-through — $78 (1x) | pass-through (port receipt) | tariff |
| CLEAN TRUCK FEE | confirm — $20 (1x) | confirm source | tariff |
| PIER PASS ADMIN FEE | flat pass-through — $50 (1x) | pass-through (port receipt) | tariff |
| PORT DETENTION | per hour after free — $70 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |

## Definitions & deviations

- **HEAVY CONTAINER FEE** — Overweight container surcharge. *(inferred)*
- **reefer** — (confirm meaning with carrier). *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **tri_axle** — (confirm meaning with carrier). *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (14%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 3 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**HEAVY CONTAINER FEE** — 3/3 sets · $250(3x) · manual

> Loads: JYCT_M108011, JYCT_M108497, JYCT_M108667

**reefer** — 3/3 sets · $150(3x) · manual

> Loads: JYCT_M108011, JYCT_M108497, JYCT_M108667

**Base Price** — 3/3 sets · $535(2x), $480(1x) · manual

> Loads: JYCT_M108011, JYCT_M108497, JYCT_M108667

**YARD STORAGE** — 3/3 sets · $45(3x) · manual

> Loads: JYCT_M108011, JYCT_M108497, JYCT_M108667

**PrePull** — 3/3 sets · $125(3x) · manual

> Loads: JYCT_M108011, JYCT_M108497, JYCT_M108667

**Chassis** — 2/3 sets · $45(2x) · manual

> Loads: JYCT_M108011, JYCT_M108667

**EMPTY LOAD STORAGE** — 2/3 sets · $50(1x), $45(1x) · 1t/1m

> Loads: JYCT_M108497, JYCT_M108667

**tri_axle** — 2/3 sets · $90(2x) · manual

> Loads: JYCT_M108497, JYCT_M108667

**DELIVERY DETENTION** — 2/3 sets · $70(2x) · manual

> Loads: JYCT_M108011, JYCT_M108497

**pier_pass** — 1/3 sets · $78(1x) · tariff

> Loads: JYCT_M108667

**CLEAN TRUCK FEE** — 1/3 sets · $20(1x) · tariff

> Loads: JYCT_M108667

**PIER PASS ADMIN FEE** — 1/3 sets · $50(1x) · tariff

> Loads: JYCT_M108667

**PORT DETENTION** — 1/3 sets · $70(2x) · manual

> Loads: JYCT_M108497

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108011 | IMPORT | PAID | $1248.7 | 7 | Juan Andres | no |
| JYCT_M108497 | IMPORT | PAID | $1724.2 | 10 | Juan Andres | no |
| JYCT_M108667 | IMPORT | DRAFT | $1708 | 11 | Juan Andres | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
