---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: GK Truckline Brokerage
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — GK Truckline Brokerage (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**56% auto.** Of 9 charge lines, **5 from the tariff** (auto-approvable) and **4 hand-keyed**. *(observed)*

- Charge sets: 1 (DRAFT:1).
- Load types: IMPORT:1.
- Biggest manual charge: **Base Price (1)** — #1 conversion target.
- Approved by: Juan Andres:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $460 (1x), $38.28 (1x) | the lane (load itself) | mixed (1t/1m) |
| YARD STORAGE | per day — $40 (1x) | yard dwell days | manual |
| Chassis | per day — $40 (1x) | days the chassis is held | manual |
| PrePull | flat — $100 (1x) | pull date vs last-free-day | manual |
| EMPTY LOAD STORAGE | per day — $50 (1x) | empty dwell days | tariff |
| pier_pass | flat pass-through — $78 (1x) | pass-through (port receipt) | tariff |
| CLEAN TRUCK FEE | confirm — $20 (1x) | confirm source | tariff |
| PIER PASS ADMIN FEE | flat pass-through — $50 (1x) | pass-through (port receipt) | tariff |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff partly wired** (56%). *(observed)*

## Validate against history

- All 1 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Base Price** — 1/1 sets · $460(1x), $38.28(1x) · 1t/1m

> Loads: JYCT_M108846

**YARD STORAGE** — 1/1 sets · $40(1x) · manual

> Loads: JYCT_M108846

**Chassis** — 1/1 sets · $40(1x) · manual

> Loads: JYCT_M108846

**PrePull** — 1/1 sets · $100(1x) · manual

> Loads: JYCT_M108846

**EMPTY LOAD STORAGE** — 1/1 sets · $50(1x) · tariff

> Loads: JYCT_M108846

**pier_pass** — 1/1 sets · $78(1x) · tariff

> Loads: JYCT_M108846

**CLEAN TRUCK FEE** — 1/1 sets · $20(1x) · tariff

> Loads: JYCT_M108846

**PIER PASS ADMIN FEE** — 1/1 sets · $50(1x) · tariff

> Loads: JYCT_M108846

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108846 | IMPORT | DRAFT | $1478 | 9 | Juan Andres | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
