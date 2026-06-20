---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: LinksMaster Logistics Co
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — LinksMaster Logistics Co (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**39% auto.** Of 18 charge lines, **7 from the tariff** (auto-approvable) and **11 hand-keyed**. *(observed)*

- Charge sets: 3 (DRAFT:1, INVOICED:1, PAID:1).
- Load types: IMPORT:3.
- Biggest manual charge: **Base Price (3)** — #1 conversion target.
- Approved by: Juan Andres:3 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $40 (2x), $50 (1x) | days the chassis is held | mixed (1t/2m) |
| Base Price | flat per move — $550 (3x) | the lane (load itself) | manual |
| PrePull | flat — $150 (1x), $125 (1x) | pull date vs last-free-day | mixed (1t/1m) |
| YARD STORAGE | per day — $50 (1x), $40 (1x) | yard dwell days | mixed (1t/1m) |
| EMPTY LOAD STORAGE | per day — $50 (1x) | empty dwell days | tariff |
| pier_pass | flat pass-through — $78 (1x) | pass-through (port receipt) | tariff |
| CLEAN TRUCK FEE | confirm — $20 (1x) | confirm source | tariff |
| PIER PASS ADMIN FEE | flat pass-through — $50 (1x) | pass-through (port receipt) | tariff |
| PORT DETENTION | per hour after free — $80 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| DELIVERY DETENTION | per hour after free — $80 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| PORT DRY RUN | flat — $50 (1x) | trouble-call evidence | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **PORT DRY RUN** — Dry run / trouble call. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (39%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 3 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 3/3 sets · $40(2x), $50(1x) · 1t/2m

> Loads: JYCT_M108454, JYCT_M108603, JYCT_M108804

**Base Price** — 3/3 sets · $550(3x) · manual

> Loads: JYCT_M108454, JYCT_M108603, JYCT_M108804

**PrePull** — 2/3 sets · $150(1x), $125(1x) · 1t/1m

> Loads: JYCT_M108454, JYCT_M108804

**YARD STORAGE** — 2/3 sets · $50(1x), $40(1x) · 1t/1m

> Loads: JYCT_M108454, JYCT_M108804

**EMPTY LOAD STORAGE** — 1/3 sets · $50(1x) · tariff

> Loads: JYCT_M108804

**pier_pass** — 1/3 sets · $78(1x) · tariff

> Loads: JYCT_M108804

**CLEAN TRUCK FEE** — 1/3 sets · $20(1x) · tariff

> Loads: JYCT_M108804

**PIER PASS ADMIN FEE** — 1/3 sets · $50(1x) · tariff

> Loads: JYCT_M108804

**PORT DETENTION** — 1/3 sets · $80(2x) · manual

> Loads: JYCT_M108603

**DELIVERY DETENTION** — 1/3 sets · $80(1x) · manual

> Loads: JYCT_M108454

**PORT DRY RUN** — 1/3 sets · $50(1x) · manual

> Loads: JYCT_M108454

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108454 | IMPORT | PAID | $885 | 6 | Juan Andres | no |
| JYCT_M108603 | IMPORT | INVOICED | $590 | 4 | Juan Andres | no |
| JYCT_M108804 | IMPORT | DRAFT | $998 | 8 | Juan Andres | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
