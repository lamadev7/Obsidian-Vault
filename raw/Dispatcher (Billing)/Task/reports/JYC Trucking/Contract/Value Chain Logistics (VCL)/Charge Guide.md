---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Value Chain Logistics (VCL)
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Value Chain Logistics (VCL) (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 50 charge lines, **0 from the tariff** (auto-approvable) and **50 hand-keyed**. *(observed)*

- Charge sets: 5 (PAID:4, INVOICED:1).
- Load types: IMPORT:5.
- Biggest manual charge: **PrePull (5)** — #1 conversion target.
- Approved by: Juan Andres:5 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| PrePull | flat — $150 (5x) | pull date vs last-free-day | manual |
| Base Price | flat per move — $625 (4x), $1050 (1x) | the lane (load itself) | manual |
| Chassis | per day — $40 (5x) | days the chassis is held | manual |
| YARD STORAGE | per day — $40 (5x) | yard dwell days | manual |
| PIER PASS ADMIN FEE | flat pass-through — $25 (4x) | pass-through (port receipt) | manual |
| pier_pass | flat pass-through — $78 (4x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $20 (4x) | confirm source | manual |
| EMPTY LOAD STORAGE | per day — $40 (4x) | empty dwell days | manual |
| DEMURRAGE ADMIN FEE | per day pass-through — $25 (3x) | terminal statement | manual |
| demurrage | per day pass-through — $285 (2x), $570 (1x) | terminal statement | manual |
| WEEKEND DELIVERY | confirm — $100 (3x) | confirm source | manual |
| DELIVERY DETENTION | per hour after free — $70 (2x), $80 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| PORT DETENTION | per hour after free — $80 (1x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| stop_off | flat per stop — $200 (1x) | count of extra stops on the load | manual |

## Definitions & deviations

- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **DEMURRAGE ADMIN FEE** — Demurrage pass-through. *(inferred)*
- **demurrage** — Demurrage pass-through. *(inferred)*
- **WEEKEND DELIVERY** — (confirm meaning with carrier). *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **stop_off** — Extra stop-off. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 5 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**PrePull** — 5/5 sets · $150(5x) · manual

> Loads: JYCT_M108290, JYCT_M108440, JYCT_M108441, JYCT_M108442, JYCT_M108443

**Base Price** — 5/5 sets · $625(4x), $1050(1x) · manual

> Loads: JYCT_M108290, JYCT_M108440, JYCT_M108441, JYCT_M108442, JYCT_M108443

**Chassis** — 5/5 sets · $40(5x) · manual

> Loads: JYCT_M108290, JYCT_M108440, JYCT_M108441, JYCT_M108442, JYCT_M108443

**YARD STORAGE** — 5/5 sets · $40(5x) · manual

> Loads: JYCT_M108290, JYCT_M108440, JYCT_M108441, JYCT_M108442, JYCT_M108443

**PIER PASS ADMIN FEE** — 4/5 sets · $25(4x) · manual

> Loads: JYCT_M108290, JYCT_M108440, JYCT_M108441, JYCT_M108443

**pier_pass** — 4/5 sets · $78(4x) · manual

> Loads: JYCT_M108290, JYCT_M108440, JYCT_M108441, JYCT_M108443

**CLEAN TRUCK FEE** — 4/5 sets · $20(4x) · manual

> Loads: JYCT_M108290, JYCT_M108440, JYCT_M108441, JYCT_M108443

**EMPTY LOAD STORAGE** — 4/5 sets · $40(4x) · manual

> Loads: JYCT_M108290, JYCT_M108440, JYCT_M108441, JYCT_M108442

**DEMURRAGE ADMIN FEE** — 3/5 sets · $25(3x) · manual

> Loads: JYCT_M108440, JYCT_M108441, JYCT_M108443

**demurrage** — 3/5 sets · $285(2x), $570(1x) · manual

> Loads: JYCT_M108440, JYCT_M108441, JYCT_M108443

**WEEKEND DELIVERY** — 3/5 sets · $100(3x) · manual

> Loads: JYCT_M108440, JYCT_M108441, JYCT_M108442

**DELIVERY DETENTION** — 2/5 sets · $70(2x), $80(1x) · manual

> Loads: JYCT_M108290, JYCT_M108443

**PORT DETENTION** — 1/5 sets · $80(1x) · manual

> Loads: JYCT_M108443

**stop_off** — 1/5 sets · $200(1x) · manual

> Loads: JYCT_M108290

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108290 | IMPORT | PAID | $1421.7 | 11 | Juan Andres | no |
| JYCT_M108440 | IMPORT | INVOICED | $1520 | 11 | Juan Andres | no |
| JYCT_M108441 | IMPORT | PAID | $1315 | 11 | Juan Andres | no |
| JYCT_M108442 | IMPORT | PAID | $1005 | 6 | Juan Andres | no |
| JYCT_M108443 | IMPORT | PAID | $1072.6 | 11 | Juan Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
