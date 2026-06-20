---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: AMERICAN CARGO INC-JENNA
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — AMERICAN CARGO INC-JENNA (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**39% auto.** Of 36 charge lines, **14 from the tariff** (auto-approvable) and **22 hand-keyed**. *(observed)*

- Charge sets: 5 (PAID:3, DRAFT:2).
- Load types: EXPORT:5.
- Biggest manual charge: **Base Price (5)** — #1 conversion target.
- Approved by: Juan Andres:5 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $545 (2x), $500 (2x) | the lane (load itself) | manual |
| Chassis | per day — $50 (5x) | days the chassis is held | mixed (2t/3m) |
| YARD STORAGE | per day — $50 (5x) | yard dwell days | mixed (2t/3m) |
| pier_pass | flat pass-through — $78 (4x) | pass-through (port receipt) | mixed (2t/2m) |
| CLEAN TRUCK FEE | confirm — $20 (4x) | confirm source | mixed (2t/2m) |
| PIER PASS ADMIN FEE | flat pass-through — $50 (2x), $25 (2x) | pass-through (port receipt) | mixed (2t/2m) |
| stop_off | flat per stop — $150 (3x) | count of extra stops on the load | manual |
| bonded_cargo_charge | flat — $150 (2x) | bonded move flag | manual |
| EMPTY LOAD STORAGE | per day — $50 (2x) | empty dwell days | tariff |
| PrePull | flat — $150 (2x) | pull date vs last-free-day | tariff |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **stop_off** — Extra stop-off. *(inferred)*
- **bonded_cargo_charge** — Bonded-cargo handling. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (39%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 5 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Base Price** — 5/5 sets · $545(2x), $500(2x), $540(1x) · manual

> Loads: JYCT_E107974, JYCT_E108152, JYCT_E108153, JYCT_E108848, JYCT_E108849

**Chassis** — 5/5 sets · $50(5x) · 2t/3m

> Loads: JYCT_E107974, JYCT_E108152, JYCT_E108153, JYCT_E108848, JYCT_E108849

**YARD STORAGE** — 5/5 sets · $50(5x) · 2t/3m

> Loads: JYCT_E107974, JYCT_E108152, JYCT_E108153, JYCT_E108848, JYCT_E108849

**pier_pass** — 4/5 sets · $78(4x) · 2t/2m

> Loads: JYCT_E108152, JYCT_E108153, JYCT_E108848, JYCT_E108849

**CLEAN TRUCK FEE** — 4/5 sets · $20(4x) · 2t/2m

> Loads: JYCT_E108152, JYCT_E108153, JYCT_E108848, JYCT_E108849

**PIER PASS ADMIN FEE** — 4/5 sets · $50(2x), $25(2x) · 2t/2m

> Loads: JYCT_E108152, JYCT_E108153, JYCT_E108848, JYCT_E108849

**stop_off** — 3/5 sets · $150(3x) · manual

> Loads: JYCT_E107974, JYCT_E108848, JYCT_E108849

**bonded_cargo_charge** — 2/5 sets · $150(2x) · manual

> Loads: JYCT_E108848, JYCT_E108849

**EMPTY LOAD STORAGE** — 2/5 sets · $50(2x) · tariff

> Loads: JYCT_E108848, JYCT_E108849

**PrePull** — 2/5 sets · $150(2x) · tariff

> Loads: JYCT_E108848, JYCT_E108849

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E107974 | EXPORT | PAID | $790 | 4 | Juan Andres | no |
| JYCT_E108152 | EXPORT | PAID | $823 | 6 | Juan Andres | no |
| JYCT_E108153 | EXPORT | PAID | $823 | 6 | Juan Andres | no |
| JYCT_E108848 | EXPORT | DRAFT | $1293 | 10 | Juan Andres | yes |
| JYCT_E108849 | EXPORT | DRAFT | $1293 | 10 | Juan Andres | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
