---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Compass Logistics International
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Compass Logistics International (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 19 charge lines, **0 from the tariff** (auto-approvable) and **19 hand-keyed**. *(observed)*

- Charge sets: 4 (PAID:4).
- Load types: EXPORT:4.
- Biggest manual charge: **Base Price (4)** — #1 conversion target.
- Approved by: Andrea:4 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $1050 (2x), $685 (1x) | the lane (load itself) | manual |
| Chassis | per day — $45 (4x) | days the chassis is held | manual |
| EMPTY LOAD STORAGE | per day — $45 (2x) | empty dwell days | manual |
| PrePull | flat — $125 (2x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $45 (2x) | yard dwell days | manual |
| WEEKEND DELIVERY | confirm — $100 (2x) | confirm source | manual |
| drop_charge | confirm — $0 (1x) | confirm source | manual |
| PORT DETENTION | per hour after free — $70 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **WEEKEND DELIVERY** — (confirm meaning with carrier). *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 4 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Base Price** — 4/4 sets · $1050(2x), $685(1x), $625(1x) · manual

> Loads: JYCT_E107730, JYCT_E108488, JYCT_E108539, JYCT_E108622

**Chassis** — 4/4 sets · $45(4x) · manual

> Loads: JYCT_E107730, JYCT_E108488, JYCT_E108539, JYCT_E108622

**EMPTY LOAD STORAGE** — 2/4 sets · $45(2x) · manual

> Loads: JYCT_E108488, JYCT_E108539

**PrePull** — 2/4 sets · $125(2x) · manual

> Loads: JYCT_E108488, JYCT_E108539

**YARD STORAGE** — 2/4 sets · $45(2x) · manual

> Loads: JYCT_E108488, JYCT_E108539

**WEEKEND DELIVERY** — 2/4 sets · $100(2x) · manual

> Loads: JYCT_E108488, JYCT_E108539

**drop_charge** — 1/4 sets · $0(1x) · manual

> Loads: JYCT_E108622

**PORT DETENTION** — 1/4 sets · $70(2x) · manual

> Loads: JYCT_E108622

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E107730 | EXPORT | PAID | $625 | 2 | Andrea | no |
| JYCT_E108488 | EXPORT | PAID | $1410 | 6 | Andrea | no |
| JYCT_E108539 | EXPORT | PAID | $1410 | 6 | Andrea | no |
| JYCT_E108622 | EXPORT | PAID | $730 | 5 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
