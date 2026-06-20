---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: ASAP Logistics, LLC
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — ASAP Logistics, LLC (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 11 charge lines, **0 from the tariff** (auto-approvable) and **11 hand-keyed**. *(observed)*

- Charge sets: 1 (PAID:1).
- Load types: IMPORT:1.
- Biggest manual charge: **Base Price (1)** — #1 conversion target.
- Approved by: Andres:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $600 (1x) | the lane (load itself) | manual |
| PrePull | flat — $125 (1x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $45 (1x) | yard dwell days | manual |
| EMPTY LOAD STORAGE | per day — $45 (1x) | empty dwell days | manual |
| Chassis | per day — $45 (1x) | days the chassis is held | manual |
| scale_load | flat — $15 (1x) | scale ticket | manual |
| reefer | confirm — $175 (1x) | confirm source | manual |
| HEAVY CONTAINER FEE | flat — $175 (1x) | overweight flag | manual |
| HIGH VALUE CARGO | confirm — $125 (1x) | confirm source | manual |
| drop_charge | confirm — $250 (1x) | confirm source | manual |
| PORT DETENTION | per hour after free — $70 (1x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*
- **reefer** — (confirm meaning with carrier). *(inferred)*
- **HEAVY CONTAINER FEE** — Overweight container surcharge. *(inferred)*
- **HIGH VALUE CARGO** — (confirm meaning with carrier). *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*

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

**Base Price** — 1/1 sets · $600(1x) · manual

> Loads: JYCT_M108105

**PrePull** — 1/1 sets · $125(1x) · manual

> Loads: JYCT_M108105

**YARD STORAGE** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108105

**EMPTY LOAD STORAGE** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108105

**Chassis** — 1/1 sets · $45(1x) · manual

> Loads: JYCT_M108105

**scale_load** — 1/1 sets · $15(1x) · manual

> Loads: JYCT_M108105

**reefer** — 1/1 sets · $175(1x) · manual

> Loads: JYCT_M108105

**HEAVY CONTAINER FEE** — 1/1 sets · $175(1x) · manual

> Loads: JYCT_M108105

**HIGH VALUE CARGO** — 1/1 sets · $125(1x) · manual

> Loads: JYCT_M108105

**drop_charge** — 1/1 sets · $250(1x) · manual

> Loads: JYCT_M108105

**PORT DETENTION** — 1/1 sets · $70(1x) · manual

> Loads: JYCT_M108105

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108105 | IMPORT | PAID | $2227.7 | 11 | Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
