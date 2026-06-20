---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: NEM SOLUTIONS LLC - PRIORITY 1
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — NEM SOLUTIONS LLC - PRIORITY 1 (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 26 charge lines, **0 from the tariff** (auto-approvable) and **26 hand-keyed**. *(observed)*

- Charge sets: 4 (PAID:4).
- Load types: IMPORT:4.
- Biggest manual charge: **Base Price (4)** — #1 conversion target.
- Approved by: Maria:4 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $500 (2x), $1 (2x) | the lane (load itself) | manual |
| flip charge | flat — $0 (2x), $220 (2x) | flip move evidence | manual |
| FLIP CHARGE ADMIN FEE | flat — $0 (2x), $50 (2x) | flip move evidence | manual |
| Chassis | per day — $40 (4x) | days the chassis is held | manual |
| PrePull | flat — $200 (2x), $1 (1x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $40 (3x) | yard dwell days | manual |
| EMPTY LOAD STORAGE | per day — $40 (2x) | empty dwell days | manual |
| PORT DETENTION | per hour after free — $70 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **flip charge** — Flip charge. *(inferred)*
- **FLIP CHARGE ADMIN FEE** — Flip charge. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
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

**Base Price** — 4/4 sets · $500(2x), $1(2x) · manual

> Loads: JYCT_M108227, JYCT_M108229, JYCT_M108230, JYCT_M108231

**flip charge** — 4/4 sets · $0(2x), $220(2x) · manual

> Loads: JYCT_M108227, JYCT_M108229, JYCT_M108230, JYCT_M108231

**FLIP CHARGE ADMIN FEE** — 4/4 sets · $0(2x), $50(2x) · manual

> Loads: JYCT_M108227, JYCT_M108229, JYCT_M108230, JYCT_M108231

**Chassis** — 4/4 sets · $40(4x) · manual

> Loads: JYCT_M108227, JYCT_M108229, JYCT_M108230, JYCT_M108231

**PrePull** — 3/4 sets · $200(2x), $1(1x) · manual

> Loads: JYCT_M108227, JYCT_M108229, JYCT_M108230

**YARD STORAGE** — 3/4 sets · $40(3x) · manual

> Loads: JYCT_M108227, JYCT_M108229, JYCT_M108230

**EMPTY LOAD STORAGE** — 2/4 sets · $40(2x) · manual

> Loads: JYCT_M108230, JYCT_M108231

**PORT DETENTION** — 2/4 sets · $70(2x) · manual

> Loads: JYCT_M108227, JYCT_M108229

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108227 | IMPORT | PAID | $1224.5 | 7 | Maria | no |
| JYCT_M108229 | IMPORT | PAID | $1158 | 7 | Maria | no |
| JYCT_M108230 | IMPORT | PAID | $1210 | 7 | Maria | no |
| JYCT_M108231 | IMPORT | PAID | $930 | 5 | Maria | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
