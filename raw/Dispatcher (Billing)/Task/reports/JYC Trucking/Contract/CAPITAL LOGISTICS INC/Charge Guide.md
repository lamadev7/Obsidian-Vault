---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: CAPITAL LOGISTICS INC
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — CAPITAL LOGISTICS INC (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 38 charge lines, **0 from the tariff** (auto-approvable) and **38 hand-keyed**. *(observed)*

- Charge sets: 4 (INVOICED:3, REBILLING:1).
- Load types: IMPORT:4.
- Biggest manual charge: **PORT DETENTION (8)** — #1 conversion target.
- Approved by: Andres:4 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $40 (4x) | days the chassis is held | manual |
| PrePull | flat — $150 (2x), $125 (2x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $40 (4x) | yard dwell days | manual |
| CLEAN TRUCK FEE | confirm — $20 (4x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $25 (4x) | pass-through (port receipt) | manual |
| Base Price | flat per move — $945 (4x) | the lane (load itself) | manual |
| PORT DETENTION | per hour after free — $75 (8x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| DELIVERY DETENTION | per hour after free — $75 (3x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| EMPTY LOAD STORAGE | per day — $40 (2x) | empty dwell days | manual |
| WEEKEND PICK-UP | confirm — $100 (1x) | confirm source | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **WEEKEND PICK-UP** — (confirm meaning with carrier). *(inferred)*

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

**Chassis** — 4/4 sets · $40(4x) · manual

> Loads: JYCT_M108694, JYCT_M108696, JYCT_M108737, JYCT_M108813

**PrePull** — 4/4 sets · $150(2x), $125(2x) · manual

> Loads: JYCT_M108694, JYCT_M108696, JYCT_M108737, JYCT_M108813

**YARD STORAGE** — 4/4 sets · $40(4x) · manual

> Loads: JYCT_M108694, JYCT_M108696, JYCT_M108737, JYCT_M108813

**CLEAN TRUCK FEE** — 4/4 sets · $20(4x) · manual

> Loads: JYCT_M108694, JYCT_M108696, JYCT_M108737, JYCT_M108813

**PIER PASS ADMIN FEE** — 4/4 sets · $25(4x) · manual

> Loads: JYCT_M108694, JYCT_M108696, JYCT_M108737, JYCT_M108813

**Base Price** — 4/4 sets · $945(4x) · manual

> Loads: JYCT_M108694, JYCT_M108696, JYCT_M108737, JYCT_M108813

**PORT DETENTION** — 4/4 sets · $75(8x) · manual

> Loads: JYCT_M108694, JYCT_M108696, JYCT_M108737, JYCT_M108813

**DELIVERY DETENTION** — 3/4 sets · $75(3x) · manual

> Loads: JYCT_M108694, JYCT_M108696, JYCT_M108813

**EMPTY LOAD STORAGE** — 2/4 sets · $40(2x) · manual

> Loads: JYCT_M108694, JYCT_M108737

**WEEKEND PICK-UP** — 1/4 sets · $100(1x) · manual

> Loads: JYCT_M108737

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108694 | IMPORT | INVOICED | $1167.25 | 10 | Andres | no |
| JYCT_M108696 | IMPORT | INVOICED | $1121.25 | 9 | Andres | no |
| JYCT_M108737 | IMPORT | REBILLING | $1110 | 10 | Andres | no |
| JYCT_M108813 | IMPORT | INVOICED | $1146 | 9 | Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
