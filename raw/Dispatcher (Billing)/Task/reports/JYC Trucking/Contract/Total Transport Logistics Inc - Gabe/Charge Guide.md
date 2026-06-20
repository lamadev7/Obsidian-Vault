---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Total Transport Logistics Inc - Gabe
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Total Transport Logistics Inc - Gabe (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 6 charge lines, **0 from the tariff** (auto-approvable) and **6 hand-keyed**. *(observed)*

- Charge sets: 1 (PAID:1).
- Load types: IMPORT:1.
- Biggest manual charge: **flip charge (1)** — #1 conversion target.
- Approved by: Andrea:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| flip charge | flat — $240 (1x) | flip move evidence | manual |
| Base Price | flat per move — $450 (1x) | the lane (load itself) | manual |
| PrePull | flat — $130 (1x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $45 (1x) | yard dwell days | manual |
| Chassis | per day — $45 (1x) | days the chassis is held | manual |
| PORT DETENTION | per hour after free — $70 (1x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |

## Definitions & deviations

- **flip charge** — Flip charge. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*

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

**flip charge** — 1/1 sets · $240(1x) · manual

> Loads: JYCT_M108496

**Base Price** — 1/1 sets · $450(1x) · manual

> Loads: JYCT_M108496

**PrePull** — 1/1 sets · $130(1x) · manual

> Loads: JYCT_M108496

**YARD STORAGE** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108496

**Chassis** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108496

**PORT DETENTION** — 1/1 sets · $70(1x) · manual

> Loads: JYCT_M108496

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108496 | IMPORT | PAID | $1192.6 | 6 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
