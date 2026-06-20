---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: C-AIR CUSTOMHOUSE BROKERS
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — C-AIR CUSTOMHOUSE BROKERS (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 6 charge lines, **0 from the tariff** (auto-approvable) and **6 hand-keyed**. *(observed)*

- Charge sets: 1 (PAID:1).
- Load types: IMPORT:1.
- Biggest manual charge: **Chassis (1)** — #1 conversion target.
- Approved by: Luciano:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $50 (1x) | days the chassis is held | manual |
| PrePull | flat — $125 (1x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $50 (1x) | yard dwell days | manual |
| Base Price | flat per move — $500 (1x) | the lane (load itself) | manual |
| PORT DETENTION | per hour after free — $70 (1x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| drop_charge | confirm — $225 (1x) | confirm source | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*

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

**Chassis** — 1/1 sets · $50(1x) · manual

> Loads: JYCT_M107794

**PrePull** — 1/1 sets · $125(1x) · manual

> Loads: JYCT_M107794

**YARD STORAGE** — 1/1 sets · $50(1x) · manual

> Loads: JYCT_M107794

**Base Price** — 1/1 sets · $500(1x) · manual

> Loads: JYCT_M107794

**PORT DETENTION** — 1/1 sets · $70(1x) · manual

> Loads: JYCT_M107794

**drop_charge** — 1/1 sets · $225(1x) · manual

> Loads: JYCT_M107794

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107794 | IMPORT | PAID | $1662.1 | 6 | Luciano | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
