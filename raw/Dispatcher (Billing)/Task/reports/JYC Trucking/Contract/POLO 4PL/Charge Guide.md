---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: POLO 4PL
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — POLO 4PL (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**40% auto.** Of 20 charge lines, **8 from the tariff** (auto-approvable) and **12 hand-keyed**. *(observed)*

- Charge sets: 2 (DRAFT:2).
- Load types: IMPORT:2.
- Biggest manual charge: **Base Price (2)** — #1 conversion target.
- Approved by: Juan Andres:2 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $1700 (1x), $1800 (1x) | the lane (load itself) | manual |
| PrePull | flat — $150 (2x) | pull date vs last-free-day | manual |
| PORT DETENTION | per hour after free — $70 (1x), $75 (1x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| DELIVERY DETENTION | per hour after free — $70 (1x), $75 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| YARD STORAGE | per day — $35 (2x) | yard dwell days | manual |
| EMPTY LOAD STORAGE | per day — $50 (2x) | empty dwell days | tariff |
| pier_pass | flat pass-through — $78 (2x) | pass-through (port receipt) | tariff |
| CLEAN TRUCK FEE | confirm — $20 (2x) | confirm source | tariff |
| PIER PASS ADMIN FEE | flat pass-through — $50 (2x) | pass-through (port receipt) | tariff |
| Chassis | per day — $35 (1x) | days the chassis is held | manual |
| tri_axle | confirm — $85 (1x) | confirm source | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **tri_axle** — (confirm meaning with carrier). *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (40%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 2 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Base Price** — 2/2 sets · $1700(1x), $1800(1x) · manual

> Loads: JYCT_M108705, JYCT_M108706

**PrePull** — 2/2 sets · $150(2x) · manual

> Loads: JYCT_M108705, JYCT_M108706

**PORT DETENTION** — 2/2 sets · $70(1x), $75(1x) · manual

> Loads: JYCT_M108705, JYCT_M108706

**DELIVERY DETENTION** — 2/2 sets · $70(1x), $75(1x) · manual

> Loads: JYCT_M108705, JYCT_M108706

**YARD STORAGE** — 2/2 sets · $35(2x) · manual

> Loads: JYCT_M108705, JYCT_M108706

**EMPTY LOAD STORAGE** — 2/2 sets · $50(2x) · tariff

> Loads: JYCT_M108705, JYCT_M108706

**pier_pass** — 2/2 sets · $78(2x) · tariff

> Loads: JYCT_M108705, JYCT_M108706

**CLEAN TRUCK FEE** — 2/2 sets · $20(2x) · tariff

> Loads: JYCT_M108705, JYCT_M108706

**PIER PASS ADMIN FEE** — 2/2 sets · $50(2x) · tariff

> Loads: JYCT_M108705, JYCT_M108706

**Chassis** — 1/2 sets · $35(1x) · manual

> Loads: JYCT_M108706

**tri_axle** — 1/2 sets · $85(1x) · manual

> Loads: JYCT_M108705

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108705 | IMPORT | DRAFT | $1998 | 10 | Juan Andres | yes |
| JYCT_M108706 | IMPORT | DRAFT | $1898 | 10 | Juan Andres | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
