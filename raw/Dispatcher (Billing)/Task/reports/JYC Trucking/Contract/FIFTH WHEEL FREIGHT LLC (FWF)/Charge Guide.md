---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: FIFTH WHEEL FREIGHT LLC (FWF)
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — FIFTH WHEEL FREIGHT LLC (FWF) (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**38% auto.** Of 8 charge lines, **3 from the tariff** (auto-approvable) and **5 hand-keyed**. *(observed)*

- Charge sets: 1 (DRAFT:1).
- Load types: IMPORT:1.
- Biggest manual charge: **Chassis (1)** — #1 conversion target.
- Approved by: Juan Andres:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $45 (1x) | days the chassis is held | manual |
| EMPTY LOAD STORAGE | per day — $45 (1x) | empty dwell days | manual |
| PrePull | flat — $100 (1x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $45 (1x) | yard dwell days | manual |
| pier_pass | flat pass-through — $78 (1x) | pass-through (port receipt) | tariff |
| CLEAN TRUCK FEE | confirm — $20 (1x) | confirm source | tariff |
| PIER PASS ADMIN FEE | flat pass-through — $50 (1x) | pass-through (port receipt) | tariff |
| Base Price | flat per move — $680 (1x) | the lane (load itself) | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (38%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 1 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108780

**EMPTY LOAD STORAGE** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108780

**PrePull** — 1/1 sets · $100(1x) · manual

> Loads: JYCT_M108780

**YARD STORAGE** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108780

**pier_pass** — 1/1 sets · $78(1x) · tariff

> Loads: JYCT_M108780

**CLEAN TRUCK FEE** — 1/1 sets · $20(1x) · tariff

> Loads: JYCT_M108780

**PIER PASS ADMIN FEE** — 1/1 sets · $50(1x) · tariff

> Loads: JYCT_M108780

**Base Price** — 1/1 sets · $680(1x) · manual

> Loads: JYCT_M108780

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108780 | IMPORT | DRAFT | $1063 | 8 | Juan Andres | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
