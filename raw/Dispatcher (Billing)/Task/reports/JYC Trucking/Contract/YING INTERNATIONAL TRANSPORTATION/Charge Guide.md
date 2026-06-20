---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: YING INTERNATIONAL TRANSPORTATION
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — YING INTERNATIONAL TRANSPORTATION (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 83 charge lines, **0 from the tariff** (auto-approvable) and **83 hand-keyed**. *(observed)*

- Charge sets: 18 (PAID:18).
- Load types: IMPORT:18.
- Biggest manual charge: **Chassis (18)** — #1 conversion target.
- Approved by: Miguel:17, Andres:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $40 (17x), $60 (1x) | days the chassis is held | manual |
| YARD STORAGE | per day — $40 (18x) | yard dwell days | manual |
| Base Price | flat per move — $750 (17x), $1790 (1x) | the lane (load itself) | manual |
| PrePull | flat — $100 (15x), $150 (1x) | pull date vs last-free-day | manual |
| EMPTY LOAD STORAGE | per day — $40 (8x) | empty dwell days | manual |
| DELIVERY DETENTION | per hour after free — $70 (4x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| scale_load | flat — $15 (1x) | scale ticket | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 18 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 18/18 sets · $40(17x), $60(1x) · manual

> Loads: JYCT_M107480, JYCT_M107481, JYCT_M107482, JYCT_M107483, JYCT_M107484, JYCT_M107485, JYCT_M107486, JYCT_M107487, JYCT_M107488, JYCT_M107489, JYCT_M107490, JYCT_M107491, JYCT_M107492, JYCT_M107493, JYCT_M107494, JYCT_M107495, JYCT_M107496, JYCT_M108098

**YARD STORAGE** — 18/18 sets · $40(18x) · manual

> Loads: JYCT_M107480, JYCT_M107481, JYCT_M107482, JYCT_M107483, JYCT_M107484, JYCT_M107485, JYCT_M107486, JYCT_M107487, JYCT_M107488, JYCT_M107489, JYCT_M107490, JYCT_M107491, JYCT_M107492, JYCT_M107493, JYCT_M107494, JYCT_M107495, JYCT_M107496, JYCT_M108098

**Base Price** — 18/18 sets · $750(17x), $1790(1x) · manual

> Loads: JYCT_M107480, JYCT_M107481, JYCT_M107482, JYCT_M107483, JYCT_M107484, JYCT_M107485, JYCT_M107486, JYCT_M107487, JYCT_M107488, JYCT_M107489, JYCT_M107490, JYCT_M107491, JYCT_M107492, JYCT_M107493, JYCT_M107494, JYCT_M107495, JYCT_M107496, JYCT_M108098

**PrePull** — 16/18 sets · $100(15x), $150(1x) · manual

> Loads: JYCT_M107480, JYCT_M107481, JYCT_M107482, JYCT_M107483, JYCT_M107484, JYCT_M107485, JYCT_M107486, JYCT_M107487, JYCT_M107488, JYCT_M107489, JYCT_M107490, JYCT_M107491, JYCT_M107493, JYCT_M107494, JYCT_M107495, JYCT_M108098

**EMPTY LOAD STORAGE** — 8/18 sets · $40(8x) · manual

> Loads: JYCT_M107481, JYCT_M107482, JYCT_M107483, JYCT_M107484, JYCT_M107485, JYCT_M107486, JYCT_M107494, JYCT_M108098

**DELIVERY DETENTION** — 4/18 sets · $70(4x) · manual

> Loads: JYCT_M107480, JYCT_M107488, JYCT_M107491, JYCT_M107493

**scale_load** — 1/18 sets · $15(1x) · manual

> Loads: JYCT_M108098

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107480 | IMPORT | PAID | $1204.2 | 5 | Miguel | no |
| JYCT_M107481 | IMPORT | PAID | $1490 | 5 | Miguel | no |
| JYCT_M107482 | IMPORT | PAID | $1290 | 5 | Miguel | no |
| JYCT_M107483 | IMPORT | PAID | $1450 | 5 | Miguel | no |
| JYCT_M107484 | IMPORT | PAID | $1410 | 5 | Miguel | no |
| JYCT_M107485 | IMPORT | PAID | $1410 | 5 | Miguel | no |
| JYCT_M107486 | IMPORT | PAID | $1450 | 5 | Miguel | no |
| JYCT_M107487 | IMPORT | PAID | $970 | 4 | Miguel | no |
| JYCT_M107488 | IMPORT | PAID | $1028.1 | 5 | Miguel | no |
| JYCT_M107489 | IMPORT | PAID | $970 | 4 | Miguel | no |
| JYCT_M107490 | IMPORT | PAID | $890 | 4 | Miguel | no |
| JYCT_M107491 | IMPORT | PAID | $1176.2 | 5 | Miguel | no |
| JYCT_M107492 | IMPORT | PAID | $790 | 3 | Miguel | no |
| JYCT_M107493 | IMPORT | PAID | $1165.7 | 5 | Miguel | no |
| JYCT_M107494 | IMPORT | PAID | $970 | 5 | Miguel | no |
| JYCT_M107495 | IMPORT | PAID | $1090 | 4 | Miguel | no |
| JYCT_M107496 | IMPORT | PAID | $790 | 3 | Miguel | no |
| JYCT_M108098 | IMPORT | PAID | $2085 | 6 | Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
