---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Nationwide Transport Service - Justin
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Nationwide Transport Service - Justin (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 7 charge lines, **0 from the tariff** (auto-approvable) and **7 hand-keyed**. *(observed)*

- Charge sets: 1 (INVOICED:1).
- Load types: EXPORT:1.
- Biggest manual charge: **Chassis (1)** — #1 conversion target.
- Approved by: Andrea:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $45 (1x) | days the chassis is held | manual |
| pier_pass | flat pass-through — $78 (1x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $20 (1x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $50 (1x) | pass-through (port receipt) | manual |
| Base Price | flat per move — $600 (1x) | the lane (load itself) | manual |
| drop_charge | confirm — $450 (1x) | confirm source | manual |
| scale_load | flat — $15 (1x) | scale ticket | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*

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

> Loads: JYCT_E108501

**pier_pass** — 1/1 sets · $78(1x) · manual

> Loads: JYCT_E108501

**CLEAN TRUCK FEE** — 1/1 sets · $20(1x) · manual

> Loads: JYCT_E108501

**PIER PASS ADMIN FEE** — 1/1 sets · $50(1x) · manual

> Loads: JYCT_E108501

**Base Price** — 1/1 sets · $600(1x) · manual

> Loads: JYCT_E108501

**drop_charge** — 1/1 sets · $450(1x) · manual

> Loads: JYCT_E108501

**scale_load** — 1/1 sets · $15(1x) · manual

> Loads: JYCT_E108501

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E108501 | EXPORT | INVOICED | $1438 | 7 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
