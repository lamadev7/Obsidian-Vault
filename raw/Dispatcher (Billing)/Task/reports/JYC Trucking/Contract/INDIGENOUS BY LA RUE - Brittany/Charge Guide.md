---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: INDIGENOUS BY LA RUE - Brittany
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — INDIGENOUS BY LA RUE - Brittany (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 6 charge lines, **0 from the tariff** (auto-approvable) and **6 hand-keyed**. *(observed)*

- Charge sets: 1 (INVOICED:1).
- Load types: IMPORT:1.
- Biggest manual charge: **DELIVERY DETENTION (1)** — #1 conversion target.
- Approved by: Andrea:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| DELIVERY DETENTION | per hour after free — $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| Base Price | flat per move — $950 (1x) | the lane (load itself) | manual |
| scale_load | flat — $15 (1x) | scale ticket | manual |
| PrePull | flat — $130 (1x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $45 (1x) | yard dwell days | manual |
| Chassis | per day — $45 (1x) | days the chassis is held | manual |

## Definitions & deviations

- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*

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

**DELIVERY DETENTION** — 1/1 sets · $70(1x) · manual

> Loads: JYCT_M108479

**Base Price** — 1/1 sets · $950(1x) · manual

> Loads: JYCT_M108479

**scale_load** — 1/1 sets · $15(1x) · manual

> Loads: JYCT_M108479

**PrePull** — 1/1 sets · $130(1x) · manual

> Loads: JYCT_M108479

**YARD STORAGE** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108479

**Chassis** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108479

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108479 | IMPORT | INVOICED | $1284.4 | 6 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
