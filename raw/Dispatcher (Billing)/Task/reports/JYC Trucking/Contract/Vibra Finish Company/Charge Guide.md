---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Vibra Finish Company
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Vibra Finish Company (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 105 charge lines, **0 from the tariff** (auto-approvable) and **105 hand-keyed**. *(observed)*

- Charge sets: 13 (PAID:11, INVOICED:2).
- Load types: IMPORT:13.
- Biggest manual charge: **Chassis (13)** — #1 conversion target.
- Approved by: Andres:13 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $50 (13x) | days the chassis is held | manual |
| PrePull | flat — $150 (13x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $50 (13x) | yard dwell days | manual |
| Base Price | flat per move — $700 (12x), $815 (1x) | the lane (load itself) | manual |
| pier_pass | flat pass-through — $78 (10x), $39 (2x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $20 (10x), $10 (2x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $25 (12x) | pass-through (port receipt) | manual |
| PORT DETENTION | per hour after free — $70 (9x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| EMPTY LOAD STORAGE | per day — $50 (4x) | empty dwell days | manual |
| other | unknown — $210 (1x) | unknown | manual |
| DELIVERY DETENTION | per hour after free — $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| HEAVY CONTAINER FEE | flat — $200 (1x) | overweight flag | manual |
| scale_load | flat — $14.75 (1x) | scale ticket | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **other** — UNDEFINED line — must be identified before trust. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **HEAVY CONTAINER FEE** — Overweight container surcharge. *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 13 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 13/13 sets · $50(13x) · manual

> Loads: JYCT_M107339, JYCT_M107391, JYCT_M107647, JYCT_M107756, JYCT_M107765, JYCT_M108071, JYCT_M108182, JYCT_M108286, JYCT_M108416, JYCT_M108431, JYCT_M108433, JYCT_M108509, JYCT_M108749

**PrePull** — 13/13 sets · $150(13x) · manual

> Loads: JYCT_M107339, JYCT_M107391, JYCT_M107647, JYCT_M107756, JYCT_M107765, JYCT_M108071, JYCT_M108182, JYCT_M108286, JYCT_M108416, JYCT_M108431, JYCT_M108433, JYCT_M108509, JYCT_M108749

**YARD STORAGE** — 13/13 sets · $50(13x) · manual

> Loads: JYCT_M107339, JYCT_M107391, JYCT_M107647, JYCT_M107756, JYCT_M107765, JYCT_M108071, JYCT_M108182, JYCT_M108286, JYCT_M108416, JYCT_M108431, JYCT_M108433, JYCT_M108509, JYCT_M108749

**Base Price** — 13/13 sets · $700(12x), $815(1x) · manual

> Loads: JYCT_M107339, JYCT_M107391, JYCT_M107647, JYCT_M107756, JYCT_M107765, JYCT_M108071, JYCT_M108182, JYCT_M108286, JYCT_M108416, JYCT_M108431, JYCT_M108433, JYCT_M108509, JYCT_M108749

**pier_pass** — 12/13 sets · $78(10x), $39(2x) · manual

> Loads: JYCT_M107339, JYCT_M107391, JYCT_M107647, JYCT_M107756, JYCT_M107765, JYCT_M108182, JYCT_M108286, JYCT_M108416, JYCT_M108431, JYCT_M108433, JYCT_M108509, JYCT_M108749

**CLEAN TRUCK FEE** — 12/13 sets · $20(10x), $10(2x) · manual

> Loads: JYCT_M107339, JYCT_M107391, JYCT_M107647, JYCT_M107756, JYCT_M107765, JYCT_M108182, JYCT_M108286, JYCT_M108416, JYCT_M108431, JYCT_M108433, JYCT_M108509, JYCT_M108749

**PIER PASS ADMIN FEE** — 12/13 sets · $25(12x) · manual

> Loads: JYCT_M107339, JYCT_M107391, JYCT_M107647, JYCT_M107756, JYCT_M107765, JYCT_M108182, JYCT_M108286, JYCT_M108416, JYCT_M108431, JYCT_M108433, JYCT_M108509, JYCT_M108749

**PORT DETENTION** — 7/13 sets · $70(9x) · manual

> Loads: JYCT_M107391, JYCT_M108182, JYCT_M108286, JYCT_M108416, JYCT_M108431, JYCT_M108433, JYCT_M108749

**EMPTY LOAD STORAGE** — 4/13 sets · $50(4x) · manual

> Loads: JYCT_M107339, JYCT_M107765, JYCT_M108071, JYCT_M108286

**other** — 1/13 sets · $210(1x) · manual

> Loads: JYCT_M108509

**DELIVERY DETENTION** — 1/13 sets · $70(1x) · manual

> Loads: JYCT_M108416

**HEAVY CONTAINER FEE** — 1/13 sets · $200(1x) · manual

> Loads: JYCT_M108071

**scale_load** — 1/13 sets · $14.75(1x) · manual

> Loads: JYCT_M107756

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107339 | IMPORT | PAID | $1274 | 8 | Andres | no |
| JYCT_M107391 | IMPORT | PAID | $1211.5 | 8 | Andres | no |
| JYCT_M107647 | IMPORT | PAID | $1173 | 7 | Andres | no |
| JYCT_M107756 | IMPORT | PAID | $1187.75 | 8 | Andres | no |
| JYCT_M107765 | IMPORT | PAID | $1273 | 8 | Andres | no |
| JYCT_M108071 | IMPORT | PAID | $1350 | 6 | Andres | no |
| JYCT_M108182 | IMPORT | PAID | $1215.7 | 8 | Andres | no |
| JYCT_M108286 | IMPORT | PAID | $1296.2 | 10 | Andres | no |
| JYCT_M108416 | IMPORT | PAID | $1335.4 | 9 | Andres | no |
| JYCT_M108431 | IMPORT | PAID | $1187 | 8 | Andres | no |
| JYCT_M108433 | IMPORT | PAID | $1194.7 | 8 | Andres | no |
| JYCT_M108509 | IMPORT | INVOICED | $1333 | 8 | Andres | no |
| JYCT_M108749 | IMPORT | INVOICED | $1332.1 | 9 | Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
