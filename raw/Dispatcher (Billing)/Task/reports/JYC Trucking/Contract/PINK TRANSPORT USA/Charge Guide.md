---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: PINK TRANSPORT USA
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — PINK TRANSPORT USA (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 9 charge lines, **0 from the tariff** (auto-approvable) and **9 hand-keyed**. *(observed)*

- Charge sets: 2 (INVOICED:1, PAID:1).
- Load types: IMPORT:2.
- Biggest manual charge: **Base Price (2)** — #1 conversion target.
- Approved by: Andrea:2 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $550 (1x), $400 (1x) | the lane (load itself) | manual |
| PrePull | flat — $115 (1x), $100 (1x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $40 (2x) | yard dwell days | manual |
| Chassis | per day — $40 (2x) | days the chassis is held | manual |
| drop_charge | confirm — $200 (1x) | confirm source | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*

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

**Base Price** — 2/2 sets · $550(1x), $400(1x) · manual

> Loads: JYCT_M108181, JYCT_M108451

**PrePull** — 2/2 sets · $115(1x), $100(1x) · manual

> Loads: JYCT_M108181, JYCT_M108451

**YARD STORAGE** — 2/2 sets · $40(2x) · manual

> Loads: JYCT_M108181, JYCT_M108451

**Chassis** — 2/2 sets · $40(2x) · manual

> Loads: JYCT_M108181, JYCT_M108451

**drop_charge** — 1/2 sets · $200(1x) · manual

> Loads: JYCT_M108181

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108181 | IMPORT | PAID | $620 | 5 | Andrea | no |
| JYCT_M108451 | IMPORT | INVOICED | $825 | 4 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
