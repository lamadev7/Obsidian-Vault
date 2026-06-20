---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Southland Brokerage Company, Inc
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Southland Brokerage Company, Inc (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 13 charge lines, **0 from the tariff** (auto-approvable) and **13 hand-keyed**. *(observed)*

- Charge sets: 1 (INVOICED:1).
- Load types: IMPORT:1.
- Biggest manual charge: **PORT DETENTION (2)** — #1 conversion target.
- Approved by: Juan Andres:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| other | unknown — $25 (1x) | unknown | manual |
| Base Price | flat per move — $550 (1x) | the lane (load itself) | manual |
| PORT DETENTION | per hour after free — $75 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| CLEAN TRUCK FEE | confirm — $20 (1x) | confirm source | manual |
| pier_pass | flat pass-through — $78 (1x) | pass-through (port receipt) | manual |
| PIER PASS ADMIN FEE | flat pass-through — $50 (1x) | pass-through (port receipt) | manual |
| DELIVERY DETENTION | per hour after free — $75 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| PrePull | flat — $135 (1x) | pull date vs last-free-day | manual |
| Chassis | per day — $45 (1x) | days the chassis is held | manual |
| YARD STORAGE | per day — $45 (1x) | yard dwell days | manual |
| EMPTY LOAD STORAGE | per day — $45 (1x) | empty dwell days | manual |
| transload | flat — $695 (1x) | transload job | manual |

## Definitions & deviations

- **other** — UNDEFINED line — must be identified before trust. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **transload** — Cross-dock/transload service. *(inferred)*

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

**other** — 1/1 sets · $25(1x) · manual

> Loads: JYCT_M108652

**Base Price** — 1/1 sets · $550(1x) · manual

> Loads: JYCT_M108652

**PORT DETENTION** — 1/1 sets · $75(2x) · manual

> Loads: JYCT_M108652

**CLEAN TRUCK FEE** — 1/1 sets · $20(1x) · manual

> Loads: JYCT_M108652

**pier_pass** — 1/1 sets · $78(1x) · manual

> Loads: JYCT_M108652

**PIER PASS ADMIN FEE** — 1/1 sets · $50(1x) · manual

> Loads: JYCT_M108652

**DELIVERY DETENTION** — 1/1 sets · $75(1x) · manual

> Loads: JYCT_M108652

**PrePull** — 1/1 sets · $135(1x) · manual

> Loads: JYCT_M108652

**Chassis** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108652

**YARD STORAGE** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108652

**EMPTY LOAD STORAGE** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108652

**transload** — 1/1 sets · $695(1x) · manual

> Loads: JYCT_M108652

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108652 | IMPORT | INVOICED | $2519 | 13 | Juan Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
