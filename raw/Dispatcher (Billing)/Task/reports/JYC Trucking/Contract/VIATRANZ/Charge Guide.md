---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: VIATRANZ
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — VIATRANZ (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 12 charge lines, **0 from the tariff** (auto-approvable) and **12 hand-keyed**. *(observed)*

- Charge sets: 4 (PAID:4).
- Load types: IMPORT:4.
- Biggest manual charge: **Chassis (4)** — #1 conversion target.
- Approved by: Fernando:3, Andrea:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $40 (4x) | days the chassis is held | manual |
| Base Price | flat per move — $725 (4x) | the lane (load itself) | manual |
| EMPTY LOAD STORAGE | per day — $40 (2x) | empty dwell days | manual |
| YARD STORAGE | per day — $40 (2x) | yard dwell days | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*

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

**Chassis** — 4/4 sets · $40(4x) · manual

> Loads: JYCT_M107459, JYCT_M107465, JYCT_M107466, JYCT_M107467

**Base Price** — 4/4 sets · $725(4x) · manual

> Loads: JYCT_M107459, JYCT_M107465, JYCT_M107466, JYCT_M107467

**EMPTY LOAD STORAGE** — 2/4 sets · $40(2x) · manual

> Loads: JYCT_M107465, JYCT_M107467

**YARD STORAGE** — 2/4 sets · $40(2x) · manual

> Loads: JYCT_M107459, JYCT_M107466

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107459 | IMPORT | PAID | $725 | 3 | Andrea | no |
| JYCT_M107465 | IMPORT | PAID | $725 | 3 | Fernando | no |
| JYCT_M107466 | IMPORT | PAID | $725 | 3 | Fernando | no |
| JYCT_M107467 | IMPORT | PAID | $725 | 3 | Fernando | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
