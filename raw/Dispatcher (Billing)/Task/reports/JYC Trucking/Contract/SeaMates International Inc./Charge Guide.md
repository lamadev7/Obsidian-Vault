---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: SeaMates International Inc.
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — SeaMates International Inc. (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 7 charge lines, **0 from the tariff** (auto-approvable) and **7 hand-keyed**. *(observed)*

- Charge sets: 1 (PAID:1).
- Load types: IMPORT:1.
- Biggest manual charge: **Chassis (1)** — #1 conversion target.
- Approved by: Juan Andres:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $45 (1x) | days the chassis is held | manual |
| PrePull | flat — $125 (1x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $45 (1x) | yard dwell days | manual |
| Base Price | flat per move — $1200 (1x) | the lane (load itself) | manual |
| flip charge | flat — $220 (1x) | flip move evidence | manual |
| FLIP CHARGE ADMIN FEE | flat — $25 (1x) | flip move evidence | manual |
| DELIVERY DETENTION | per hour after free — $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **flip charge** — Flip charge. *(inferred)*
- **FLIP CHARGE ADMIN FEE** — Flip charge. *(inferred)*
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

> Loads: JYCT_M107814

**PrePull** — 1/1 sets · $125(1x) · manual

> Loads: JYCT_M107814

**YARD STORAGE** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M107814

**Base Price** — 1/1 sets · $1200(1x) · manual

> Loads: JYCT_M107814

**flip charge** — 1/1 sets · $220(1x) · manual

> Loads: JYCT_M107814

**FLIP CHARGE ADMIN FEE** — 1/1 sets · $25(1x) · manual

> Loads: JYCT_M107814

**DELIVERY DETENTION** — 1/1 sets · $70(1x) · manual

> Loads: JYCT_M107814

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107814 | IMPORT | PAID | $1831.2 | 7 | Juan Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
