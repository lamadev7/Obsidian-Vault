---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Ronco Freight International Inc
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Ronco Freight International Inc (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 5 charge lines, **0 from the tariff** (auto-approvable) and **5 hand-keyed**. *(observed)*

- Charge sets: 1 (PAID:1).
- Load types: EXPORT:1.
- Biggest manual charge: **Base Price (1)** — #1 conversion target.
- Approved by: Juan Andres:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $1815 (1x) | the lane (load itself) | manual |
| PrePull | flat — $125 (1x) | pull date vs last-free-day | manual |
| EMPTY LOAD STORAGE | per day — $40 (1x) | empty dwell days | manual |
| Chassis | per day — $45 (1x) | days the chassis is held | manual |
| WEEKEND DELIVERY | confirm — $100 (1x) | confirm source | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **WEEKEND DELIVERY** — (confirm meaning with carrier). *(inferred)*

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

**Base Price** — 1/1 sets · $1815(1x) · manual

> Loads: JYCT_E108445

**PrePull** — 1/1 sets · $125(1x) · manual

> Loads: JYCT_E108445

**EMPTY LOAD STORAGE** — 1/1 sets · $40(1x) · manual

> Loads: JYCT_E108445

**Chassis** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_E108445

**WEEKEND DELIVERY** — 1/1 sets · $100(1x) · manual

> Loads: JYCT_E108445

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E108445 | EXPORT | PAID | $2215 | 5 | Juan Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
