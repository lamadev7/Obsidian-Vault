---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Lion Heart Expedited
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Lion Heart Expedited (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 12 charge lines, **0 from the tariff** (auto-approvable) and **12 hand-keyed**. *(observed)*

- Charge sets: 2 (REBILLING:1, PAID:1).
- Load types: IMPORT:2.
- Biggest manual charge: **DELIVERY DETENTION (3)** — #1 conversion target.
- Approved by: Andrea:2 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $45 (2x) | days the chassis is held | manual |
| PrePull | flat — $125 (2x) | pull date vs last-free-day | manual |
| Base Price | flat per move — $585 (1x), $970 (1x) | the lane (load itself) | manual |
| DELIVERY DETENTION | per hour after free — $80 (2x), $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| YARD STORAGE | per day — $45 (2x) | yard dwell days | manual |
| PORT DETENTION | per hour after free — $80 (1x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*

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

**Chassis** — 2/2 sets · $45(2x) · manual

> Loads: JYCT_M108165, JYCT_M108752

**PrePull** — 2/2 sets · $125(2x) · manual

> Loads: JYCT_M108165, JYCT_M108752

**Base Price** — 2/2 sets · $585(1x), $970(1x) · manual

> Loads: JYCT_M108165, JYCT_M108752

**DELIVERY DETENTION** — 2/2 sets · $80(2x), $70(1x) · manual

> Loads: JYCT_M108165, JYCT_M108752

**YARD STORAGE** — 2/2 sets · $45(2x) · manual

> Loads: JYCT_M108165, JYCT_M108752

**PORT DETENTION** — 1/2 sets · $80(1x) · manual

> Loads: JYCT_M108752

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108165 | IMPORT | PAID | $1362.5 | 5 | Andrea | no |
| JYCT_M108752 | IMPORT | REBILLING | $1049 | 7 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
