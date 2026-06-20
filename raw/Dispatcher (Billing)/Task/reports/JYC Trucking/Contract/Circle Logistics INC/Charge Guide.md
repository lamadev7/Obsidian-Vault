---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Circle Logistics INC
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Circle Logistics INC (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 9 charge lines, **0 from the tariff** (auto-approvable) and **9 hand-keyed**. *(observed)*

- Charge sets: 1 (PAID:1).
- Load types: EXPORT:1.
- Biggest manual charge: **Chassis (1)** — #1 conversion target.
- Approved by: Juan Andres:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $40 (1x) | days the chassis is held | manual |
| EMPTY LOAD STORAGE | per day — $40 (1x) | empty dwell days | manual |
| PrePull | flat — $100 (1x) | pull date vs last-free-day | manual |
| Base Price | flat per move — $1 (1x) | the lane (load itself) | manual |
| DELIVERY DETENTION | per hour after free — $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| PORT DETENTION | per hour after free — $70 (1x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| pier_pass | flat pass-through — $38.78 (1x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $10 (1x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $25 (1x) | pass-through (port receipt) | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*

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

**Chassis** — 1/1 sets · $40(1x) · manual

> Loads: JYCT_E108247

**EMPTY LOAD STORAGE** — 1/1 sets · $40(1x) · manual

> Loads: JYCT_E108247

**PrePull** — 1/1 sets · $100(1x) · manual

> Loads: JYCT_E108247

**Base Price** — 1/1 sets · $1(1x) · manual

> Loads: JYCT_E108247

**DELIVERY DETENTION** — 1/1 sets · $70(1x) · manual

> Loads: JYCT_E108247

**PORT DETENTION** — 1/1 sets · $70(1x) · manual

> Loads: JYCT_E108247

**pier_pass** — 1/1 sets · $38.78(1x) · manual

> Loads: JYCT_E108247

**CLEAN TRUCK FEE** — 1/1 sets · $10(1x) · manual

> Loads: JYCT_E108247

**PIER PASS ADMIN FEE** — 1/1 sets · $25(1x) · manual

> Loads: JYCT_E108247

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E108247 | EXPORT | PAID | $1270.48 | 9 | Juan Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
