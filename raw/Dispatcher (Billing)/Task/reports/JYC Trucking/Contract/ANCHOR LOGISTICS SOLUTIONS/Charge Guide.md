---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: ANCHOR LOGISTICS SOLUTIONS
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — ANCHOR LOGISTICS SOLUTIONS (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 19 charge lines, **0 from the tariff** (auto-approvable) and **19 hand-keyed**. *(observed)*

- Charge sets: 2 (PAID:2).
- Load types: EXPORT:1, IMPORT:1.
- Biggest manual charge: **Chassis (2)** — #1 conversion target.
- Approved by: Andres:2 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $50 (2x) | days the chassis is held | manual |
| YARD STORAGE | per day — $50 (2x) | yard dwell days | manual |
| Base Price | flat per move — $420 (1x), $450 (1x) | the lane (load itself) | manual |
| pier_pass | flat pass-through — $38 (1x), $78 (1x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $10 (1x), $20 (1x) | confirm source | manual |
| PrePull | flat — $125 (2x) | pull date vs last-free-day | manual |
| EMPTY LOAD STORAGE | per day — $50 (1x) | empty dwell days | manual |
| ADMIN FEE | confirm — $25 (1x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $25 (1x) | pass-through (port receipt) | manual |
| DELIVERY DETENTION | per hour after free — $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| transload | flat — $600 (1x) | transload job | manual |
| other | unknown — $220 (1x) | unknown | manual |
| WAREHOUSE STORGE | per day — $62.7 (1x) | warehouse dwell days | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **ADMIN FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **transload** — Cross-dock/transload service. *(inferred)*
- **other** — UNDEFINED line — must be identified before trust. *(inferred)*
- **WAREHOUSE STORGE** — Storage in the warehouse. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 2 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 2/2 sets · $50(2x) · manual

> Loads: JYCT_E108147, JYCT_M107884

**YARD STORAGE** — 2/2 sets · $50(2x) · manual

> Loads: JYCT_E108147, JYCT_M107884

**Base Price** — 2/2 sets · $420(1x), $450(1x) · manual

> Loads: JYCT_E108147, JYCT_M107884

**pier_pass** — 2/2 sets · $38(1x), $78(1x) · manual

> Loads: JYCT_E108147, JYCT_M107884

**CLEAN TRUCK FEE** — 2/2 sets · $10(1x), $20(1x) · manual

> Loads: JYCT_E108147, JYCT_M107884

**PrePull** — 2/2 sets · $125(2x) · manual

> Loads: JYCT_E108147, JYCT_M107884

**EMPTY LOAD STORAGE** — 1/2 sets · $50(1x) · manual

> Loads: JYCT_E108147

**ADMIN FEE** — 1/2 sets · $25(1x) · manual

> Loads: JYCT_E108147

**PIER PASS ADMIN FEE** — 1/2 sets · $25(1x) · manual

> Loads: JYCT_M107884

**DELIVERY DETENTION** — 1/2 sets · $70(1x) · manual

> Loads: JYCT_M107884

**transload** — 1/2 sets · $600(1x) · manual

> Loads: JYCT_M107884

**other** — 1/2 sets · $220(1x) · manual

> Loads: JYCT_M107884

**WAREHOUSE STORGE** — 1/2 sets · $62.7(1x) · manual

> Loads: JYCT_M107884

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E108147 | EXPORT | PAID | $755.5 | 8 | Andres | no |
| JYCT_M107884 | IMPORT | PAID | $1866.5 | 11 | Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
