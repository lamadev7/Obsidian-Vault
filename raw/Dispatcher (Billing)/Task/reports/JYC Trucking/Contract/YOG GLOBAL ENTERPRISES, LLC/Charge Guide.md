---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: YOG GLOBAL ENTERPRISES, LLC
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — YOG GLOBAL ENTERPRISES, LLC (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 7 charge lines, **0 from the tariff** (auto-approvable) and **7 hand-keyed**. *(observed)*

- Charge sets: 1 (PAID:1).
- Load types: IMPORT:1.
- Biggest manual charge: **Chassis (1)** — #1 conversion target.
- Approved by: Andres:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $45 (1x) | days the chassis is held | manual |
| EMPTY LOAD STORAGE | per day — $45 (1x) | empty dwell days | manual |
| YARD STORAGE | per day — $45 (1x) | yard dwell days | manual |
| PrePull | flat — $100 (1x) | pull date vs last-free-day | manual |
| Base Price | flat per move — $410 (1x) | the lane (load itself) | manual |
| HEAVY CONTAINER FEE | flat — $175 (1x) | overweight flag | manual |
| DELIVERY DETENTION | per hour after free — $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **HEAVY CONTAINER FEE** — Overweight container surcharge. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 1 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M107379

**EMPTY LOAD STORAGE** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M107379

**YARD STORAGE** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M107379

**PrePull** — 1/1 sets · $100(1x) · manual

> Loads: JYCT_M107379

**Base Price** — 1/1 sets · $410(1x) · manual

> Loads: JYCT_M107379

**HEAVY CONTAINER FEE** — 1/1 sets · $175(1x) · manual

> Loads: JYCT_M107379

**DELIVERY DETENTION** — 1/1 sets · $70(1x) · manual

> Loads: JYCT_M107379

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107379 | IMPORT | PAID | $1089 | 7 | Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
