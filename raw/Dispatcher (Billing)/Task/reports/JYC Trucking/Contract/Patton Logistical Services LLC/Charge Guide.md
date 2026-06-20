---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Patton Logistical Services LLC
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Patton Logistical Services LLC (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 40 charge lines, **0 from the tariff** (auto-approvable) and **40 hand-keyed**. *(observed)*

- Charge sets: 4 (PAID:4).
- Load types: EXPORT:4.
- Biggest manual charge: **DELIVERY DETENTION (6)** — #1 conversion target.
- Approved by: Maria:4 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $1 (2x), $1680 (2x) | the lane (load itself) | manual |
| PrePull | flat — $125 (4x) | pull date vs last-free-day | manual |
| Chassis | per day — $40 (4x) | days the chassis is held | manual |
| bonded_cargo_charge | flat — $0 (2x), $1 (2x) | bonded move flag | manual |
| DELIVERY DETENTION | per hour after free — $70 (6x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| YARD STORAGE | per day — $40 (4x) | yard dwell days | manual |
| EMPTY LOAD STORAGE | per day — $40 (4x) | empty dwell days | manual |
| Hazmat | confirm — $0 (4x) | confirm source | manual |
| PORT DETENTION | per hour after free — $70 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| dry_run | flat — $1680 (2x) | trouble-call evidence | manual |
| PORT DRY RUN | flat — $100 (2x) | trouble-call evidence | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **bonded_cargo_charge** — Bonded-cargo handling. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **Hazmat** — (confirm meaning with carrier). *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **dry_run** — Dry run / trouble call. *(inferred)*
- **PORT DRY RUN** — Dry run / trouble call. *(inferred)*

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

**Base Price** — 4/4 sets · $1(2x), $1680(2x) · manual

> Loads: JYCT_E108192, JYCT_E108197, JYCT_E108341, JYCT_E108342

**PrePull** — 4/4 sets · $125(4x) · manual

> Loads: JYCT_E108192, JYCT_E108197, JYCT_E108341, JYCT_E108342

**Chassis** — 4/4 sets · $40(4x) · manual

> Loads: JYCT_E108192, JYCT_E108197, JYCT_E108341, JYCT_E108342

**bonded_cargo_charge** — 4/4 sets · $0(2x), $1(2x) · manual

> Loads: JYCT_E108192, JYCT_E108197, JYCT_E108341, JYCT_E108342

**DELIVERY DETENTION** — 4/4 sets · $70(6x) · manual

> Loads: JYCT_E108192, JYCT_E108197, JYCT_E108341, JYCT_E108342

**YARD STORAGE** — 4/4 sets · $40(4x) · manual

> Loads: JYCT_E108192, JYCT_E108197, JYCT_E108341, JYCT_E108342

**EMPTY LOAD STORAGE** — 4/4 sets · $40(4x) · manual

> Loads: JYCT_E108192, JYCT_E108197, JYCT_E108341, JYCT_E108342

**Hazmat** — 4/4 sets · $0(4x) · manual

> Loads: JYCT_E108192, JYCT_E108197, JYCT_E108341, JYCT_E108342

**PORT DETENTION** — 2/4 sets · $70(2x) · manual

> Loads: JYCT_E108192, JYCT_E108197

**dry_run** — 2/4 sets · $1680(2x) · manual

> Loads: JYCT_E108192, JYCT_E108197

**PORT DRY RUN** — 2/4 sets · $100(2x) · manual

> Loads: JYCT_E108192, JYCT_E108197

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E108192 | EXPORT | PAID | $5038.7 | 12 | Maria | no |
| JYCT_E108197 | EXPORT | PAID | $5091.8 | 12 | Maria | no |
| JYCT_E108341 | EXPORT | PAID | $2712.5 | 8 | Maria | no |
| JYCT_E108342 | EXPORT | PAID | $2555 | 8 | Maria | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
