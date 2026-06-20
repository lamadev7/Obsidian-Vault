---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Billy - Nationwide Transportation Distribution Services
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Billy - Nationwide Transportation Distribution Services (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 10 charge lines, **0 from the tariff** (auto-approvable) and **10 hand-keyed**. *(observed)*

- Charge sets: 2 (PAID:2).
- Load types: IMPORT:2.
- Biggest manual charge: **Chassis (2)** — #1 conversion target.
- Approved by: Luciano:1, Paula:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $40 (2x) | days the chassis is held | manual |
| EMPTY LOAD STORAGE | per day — $40 (2x) | empty dwell days | manual |
| Base Price | flat per move — $660 (2x) | the lane (load itself) | manual |
| reefer | confirm — $200 (2x) | confirm source | manual |
| PrePull | flat — $90 (1x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $40 (1x) | yard dwell days | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **reefer** — (confirm meaning with carrier). *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*

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

**Chassis** — 2/2 sets · $40(2x) · manual

> Loads: JYCT_M107959, JYCT_M107993

**EMPTY LOAD STORAGE** — 2/2 sets · $40(2x) · manual

> Loads: JYCT_M107959, JYCT_M107993

**Base Price** — 2/2 sets · $660(2x) · manual

> Loads: JYCT_M107959, JYCT_M107993

**reefer** — 2/2 sets · $200(2x) · manual

> Loads: JYCT_M107959, JYCT_M107993

**PrePull** — 1/2 sets · $90(1x) · manual

> Loads: JYCT_M107959

**YARD STORAGE** — 1/2 sets · $40(1x) · manual

> Loads: JYCT_M107959

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107959 | IMPORT | PAID | $1350 | 6 | Paula | no |
| JYCT_M107993 | IMPORT | PAID | $940 | 4 | Luciano | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
