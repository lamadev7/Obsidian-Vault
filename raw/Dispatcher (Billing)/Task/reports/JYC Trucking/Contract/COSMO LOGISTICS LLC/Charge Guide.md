---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: COSMO LOGISTICS LLC
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — COSMO LOGISTICS LLC (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 16 charge lines, **0 from the tariff** (auto-approvable) and **16 hand-keyed**. *(observed)*

- Charge sets: 3 (INVOICED:3).
- Load types: EXPORT:2, IMPORT:1.
- Biggest manual charge: **Base Price (3)** — #1 conversion target.
- Approved by: Maria:3 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $850 (1x), $675 (1x) | the lane (load itself) | manual |
| Chassis | per day — $40 (3x) | days the chassis is held | manual |
| EMPTY LOAD STORAGE | per day — $40 (2x) | empty dwell days | manual |
| reefer | confirm — $0 (1x) | confirm source | manual |
| drop_charge | confirm — $220 (1x) | confirm source | manual |
| PrePull | flat — $125 (1x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $40 (1x) | yard dwell days | manual |
| PIER PASS ADMIN FEE | flat pass-through — $35 (1x) | pass-through (port receipt) | manual |
| pier_pass | flat pass-through — $78 (1x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $20 (1x) | confirm source | manual |
| DELIVERY DETENTION | per hour after free — $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **reefer** — (confirm meaning with carrier). *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 3 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Base Price** — 3/3 sets · $850(1x), $675(1x), $530(1x) · manual

> Loads: JYCT_E108408, JYCT_E108448, JYCT_M108428

**Chassis** — 3/3 sets · $40(3x) · manual

> Loads: JYCT_E108408, JYCT_E108448, JYCT_M108428

**EMPTY LOAD STORAGE** — 2/3 sets · $40(2x) · manual

> Loads: JYCT_E108408, JYCT_M108428

**reefer** — 1/3 sets · $0(1x) · manual

> Loads: JYCT_E108448

**drop_charge** — 1/3 sets · $220(1x) · manual

> Loads: JYCT_E108448

**PrePull** — 1/3 sets · $125(1x) · manual

> Loads: JYCT_E108408

**YARD STORAGE** — 1/3 sets · $40(1x) · manual

> Loads: JYCT_E108408

**PIER PASS ADMIN FEE** — 1/3 sets · $35(1x) · manual

> Loads: JYCT_E108408

**pier_pass** — 1/3 sets · $78(1x) · manual

> Loads: JYCT_E108408

**CLEAN TRUCK FEE** — 1/3 sets · $20(1x) · manual

> Loads: JYCT_E108408

**DELIVERY DETENTION** — 1/3 sets · $70(1x) · manual

> Loads: JYCT_E108408

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E108408 | EXPORT | INVOICED | $784.9 | 9 | Maria | no |
| JYCT_E108448 | EXPORT | INVOICED | $850 | 4 | Maria | no |
| JYCT_M108428 | IMPORT | INVOICED | $675 | 3 | Maria | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
