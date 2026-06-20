---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: MAC CUSTOMS BROKERAGE
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — MAC CUSTOMS BROKERAGE (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 23 charge lines, **0 from the tariff** (auto-approvable) and **23 hand-keyed**. *(observed)*

- Charge sets: 5 (PAID:5).
- Load types: IMPORT:5.
- Biggest manual charge: **PrePull (5)** — #1 conversion target.
- Approved by: Andrea:5 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| PrePull | flat — $100 (5x) | pull date vs last-free-day | manual |
| Base Price | flat per move — $550 (5x) | the lane (load itself) | manual |
| YARD STORAGE | per day — $40 (5x) | yard dwell days | manual |
| Chassis | per day — $40 (5x) | days the chassis is held | manual |
| PORT DETENTION | per hour after free — $70 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| DELIVERY DETENTION | per hour after free — $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |

## Definitions & deviations

- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*

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

**PrePull** — 5/5 sets · $100(5x) · manual

> Loads: JYCT_M107520, JYCT_M107639, JYCT_M107961, JYCT_M107999, JYCT_M108368

**Base Price** — 5/5 sets · $550(5x) · manual

> Loads: JYCT_M107520, JYCT_M107639, JYCT_M107961, JYCT_M107999, JYCT_M108368

**YARD STORAGE** — 5/5 sets · $40(5x) · manual

> Loads: JYCT_M107520, JYCT_M107639, JYCT_M107961, JYCT_M107999, JYCT_M108368

**Chassis** — 5/5 sets · $40(5x) · manual

> Loads: JYCT_M107520, JYCT_M107639, JYCT_M107961, JYCT_M107999, JYCT_M108368

**PORT DETENTION** — 2/5 sets · $70(2x) · manual

> Loads: JYCT_M107639, JYCT_M108368

**DELIVERY DETENTION** — 1/5 sets · $70(1x) · manual

> Loads: JYCT_M108368

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107520 | IMPORT | PAID | $1770 | 4 | Andrea | no |
| JYCT_M107639 | IMPORT | PAID | $702.6 | 5 | Andrea | no |
| JYCT_M107961 | IMPORT | PAID | $730 | 4 | Andrea | no |
| JYCT_M107999 | IMPORT | PAID | $730 | 4 | Andrea | no |
| JYCT_M108368 | IMPORT | PAID | $721.5 | 6 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
