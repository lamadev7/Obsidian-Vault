---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Sunny International Logistics Inc
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Sunny International Logistics Inc (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**62% auto.** Of 26 charge lines, **16 from the tariff** (auto-approvable) and **10 hand-keyed**. *(observed)*

- Charge sets: 4 (DRAFT:3, INVOICED:1).
- Load types: IMPORT:3, EXPORT:1.
- Biggest manual charge: **bonded_cargo_charge (3)** — #1 conversion target.
- Approved by: Andres:4 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| PrePull | flat — $150 (2x), $125 (1x) | pull date vs last-free-day | mixed (2t/1m) |
| Base Price | flat per move — $33.7 (2x), $575 (1x) | the lane (load itself) | mixed (2t/1m) |
| Chassis | per day — $50 (2x), $45 (1x) | days the chassis is held | mixed (2t/1m) |
| YARD STORAGE | per day — $50 (2x), $45 (1x) | yard dwell days | mixed (2t/1m) |
| bonded_cargo_charge | flat — $250 (3x) | bonded move flag | manual |
| EMPTY LOAD STORAGE | per day — $50 (2x) | empty dwell days | tariff |
| pier_pass | flat pass-through — $78 (2x) | pass-through (port receipt) | tariff |
| CLEAN TRUCK FEE | confirm — $20 (2x) | confirm source | tariff |
| PIER PASS ADMIN FEE | flat pass-through — $50 (2x) | pass-through (port receipt) | tariff |
| WEEKEND PICK-UP | confirm — $100 (1x) | confirm source | manual |
| PORT DETENTION | per hour after free — $70 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |

## Definitions & deviations

- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **bonded_cargo_charge** — Bonded-cargo handling. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **WEEKEND PICK-UP** — (confirm meaning with carrier). *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff partly wired** (62%). *(observed)*

## Validate against history

- All 4 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**PrePull** — 3/4 sets · $150(2x), $125(1x) · 2t/1m

> Loads: JYCT_M108777, JYCT_M108778, JYCT_M108779

**Base Price** — 3/4 sets · $33.7(2x), $575(1x) · 2t/1m

> Loads: JYCT_M108777, JYCT_M108778, JYCT_M108779

**Chassis** — 3/4 sets · $50(2x), $45(1x) · 2t/1m

> Loads: JYCT_M108777, JYCT_M108778, JYCT_M108779

**YARD STORAGE** — 3/4 sets · $50(2x), $45(1x) · 2t/1m

> Loads: JYCT_M108777, JYCT_M108778, JYCT_M108779

**bonded_cargo_charge** — 3/4 sets · $250(3x) · manual

> Loads: JYCT_E108776, JYCT_M108777, JYCT_M108778

**EMPTY LOAD STORAGE** — 2/4 sets · $50(2x) · tariff

> Loads: JYCT_M108777, JYCT_M108778

**pier_pass** — 2/4 sets · $78(2x) · tariff

> Loads: JYCT_M108777, JYCT_M108778

**CLEAN TRUCK FEE** — 2/4 sets · $20(2x) · tariff

> Loads: JYCT_M108777, JYCT_M108778

**PIER PASS ADMIN FEE** — 2/4 sets · $50(2x) · tariff

> Loads: JYCT_M108777, JYCT_M108778

**WEEKEND PICK-UP** — 1/4 sets · $100(1x) · manual

> Loads: JYCT_M108779

**PORT DETENTION** — 1/4 sets · $70(2x) · manual

> Loads: JYCT_M108779

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E108776 | EXPORT | DRAFT | $250 | 1 | Andres | no |
| JYCT_M108777 | IMPORT | DRAFT | $1098 | 9 | Andres | yes |
| JYCT_M108778 | IMPORT | DRAFT | $1098 | 9 | Andres | yes |
| JYCT_M108779 | IMPORT | INVOICED | $1160 | 7 | Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
