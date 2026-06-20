---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: HPL Apollo - Norgi
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — HPL Apollo - Norgi (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**11% auto.** Of 71 charge lines, **8 from the tariff** (auto-approvable) and **63 hand-keyed**. *(observed)*

- Charge sets: 9 (PAID:8, DRAFT:1).
- Load types: EXPORT:9.
- Biggest manual charge: **Chassis (8)** — #1 conversion target.
- Approved by: Andrea:9 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $45 (8x), $50 (1x) | days the chassis is held | mixed (1t/8m) |
| pier_pass | flat pass-through — $78 (8x), $77.56 (1x) | pass-through (port receipt) | mixed (1t/8m) |
| Base Price | flat per move — $450 (8x), $12.08 (1x) | the lane (load itself) | mixed (1t/8m) |
| CLEAN TRUCK FEE | confirm — $20 (9x) | confirm source | mixed (1t/8m) |
| PIER PASS ADMIN FEE | flat pass-through — $25 (8x), $50 (1x) | pass-through (port receipt) | mixed (1t/8m) |
| reefer | confirm — $150 (8x) | confirm source | manual |
| EMPTY LOAD STORAGE | per day — $45 (4x), $50 (1x) | empty dwell days | mixed (1t/4m) |
| YARD STORAGE | per day — $45 (4x), $50 (1x) | yard dwell days | mixed (1t/4m) |
| PrePull | flat — $125 (3x), $150 (1x) | pull date vs last-free-day | mixed (1t/3m) |
| DELIVERY DETENTION | per hour after free — $70 (4x) | consignee arrive/depart clock, cross-checked vs driver | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **reefer** — (confirm meaning with carrier). *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (11%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 9 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 9/9 sets · $45(8x), $50(1x) · 1t/8m

> Loads: JYCT_E107657, JYCT_E107882, JYCT_E107895, JYCT_E107896, JYCT_E107897, JYCT_E108246, JYCT_E108353, JYCT_E108406, JYCT_E108834

**pier_pass** — 9/9 sets · $78(8x), $77.56(1x) · 1t/8m

> Loads: JYCT_E107657, JYCT_E107882, JYCT_E107895, JYCT_E107896, JYCT_E107897, JYCT_E108246, JYCT_E108353, JYCT_E108406, JYCT_E108834

**Base Price** — 9/9 sets · $450(8x), $12.08(1x) · 1t/8m

> Loads: JYCT_E107657, JYCT_E107882, JYCT_E107895, JYCT_E107896, JYCT_E107897, JYCT_E108246, JYCT_E108353, JYCT_E108406, JYCT_E108834

**CLEAN TRUCK FEE** — 9/9 sets · $20(9x) · 1t/8m

> Loads: JYCT_E107657, JYCT_E107882, JYCT_E107895, JYCT_E107896, JYCT_E107897, JYCT_E108246, JYCT_E108353, JYCT_E108406, JYCT_E108834

**PIER PASS ADMIN FEE** — 9/9 sets · $25(8x), $50(1x) · 1t/8m

> Loads: JYCT_E107657, JYCT_E107882, JYCT_E107895, JYCT_E107896, JYCT_E107897, JYCT_E108246, JYCT_E108353, JYCT_E108406, JYCT_E108834

**reefer** — 8/9 sets · $150(8x) · manual

> Loads: JYCT_E107657, JYCT_E107882, JYCT_E107895, JYCT_E107896, JYCT_E107897, JYCT_E108246, JYCT_E108353, JYCT_E108406

**EMPTY LOAD STORAGE** — 5/9 sets · $45(4x), $50(1x) · 1t/4m

> Loads: JYCT_E107657, JYCT_E108246, JYCT_E108353, JYCT_E108406, JYCT_E108834

**YARD STORAGE** — 5/9 sets · $45(4x), $50(1x) · 1t/4m

> Loads: JYCT_E107895, JYCT_E107896, JYCT_E107897, JYCT_E108406, JYCT_E108834

**PrePull** — 4/9 sets · $125(3x), $150(1x) · 1t/3m

> Loads: JYCT_E108246, JYCT_E108353, JYCT_E108406, JYCT_E108834

**DELIVERY DETENTION** — 4/9 sets · $70(4x) · manual

> Loads: JYCT_E107895, JYCT_E107897, JYCT_E108353, JYCT_E108406

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E107657 | EXPORT | PAID | $858 | 7 | Andrea | no |
| JYCT_E107882 | EXPORT | PAID | $768 | 6 | Andrea | no |
| JYCT_E107895 | EXPORT | PAID | $906.1 | 8 | Andrea | no |
| JYCT_E107896 | EXPORT | PAID | $903 | 7 | Andrea | no |
| JYCT_E107897 | EXPORT | PAID | $932.7 | 8 | Andrea | no |
| JYCT_E108246 | EXPORT | PAID | $1028 | 8 | Andrea | no |
| JYCT_E108353 | EXPORT | PAID | $1086.1 | 9 | Andrea | no |
| JYCT_E108406 | EXPORT | PAID | $1062.56 | 10 | Andrea | no |
| JYCT_E108834 | EXPORT | DRAFT | $948 | 8 | Andrea | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
