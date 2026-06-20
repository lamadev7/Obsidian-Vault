---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Clean Earth Logistics - Paul
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Clean Earth Logistics - Paul (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 15 charge lines, **0 from the tariff** (auto-approvable) and **15 hand-keyed**. *(observed)*

- Charge sets: 2 (PAID:2).
- Load types: IMPORT:2.
- Biggest manual charge: **Chassis (2)** — #1 conversion target.
- Approved by: Andrea:2 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $45 (2x) | days the chassis is held | manual |
| EMPTY LOAD STORAGE | per day — $40 (2x) | empty dwell days | manual |
| pier_pass | flat pass-through — $78 (2x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $20 (2x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $25 (2x) | pass-through (port receipt) | manual |
| Base Price | flat per move — $450 (2x) | the lane (load itself) | manual |
| transload | flat — $550 (2x) | transload job | manual |
| other | unknown — $6 (1x) | unknown | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **transload** — Cross-dock/transload service. *(inferred)*
- **other** — UNDEFINED line — must be identified before trust. *(inferred)*

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

**Chassis** — 2/2 sets · $45(2x) · manual

> Loads: JYCT_M107403, JYCT_M107519

**EMPTY LOAD STORAGE** — 2/2 sets · $40(2x) · manual

> Loads: JYCT_M107403, JYCT_M107519

**pier_pass** — 2/2 sets · $78(2x) · manual

> Loads: JYCT_M107403, JYCT_M107519

**CLEAN TRUCK FEE** — 2/2 sets · $20(2x) · manual

> Loads: JYCT_M107403, JYCT_M107519

**PIER PASS ADMIN FEE** — 2/2 sets · $25(2x) · manual

> Loads: JYCT_M107403, JYCT_M107519

**Base Price** — 2/2 sets · $450(2x) · manual

> Loads: JYCT_M107403, JYCT_M107519

**transload** — 2/2 sets · $550(2x) · manual

> Loads: JYCT_M107403, JYCT_M107519

**other** — 1/2 sets · $6(1x) · manual

> Loads: JYCT_M107403

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107403 | IMPORT | PAID | $1299 | 8 | Andrea | no |
| JYCT_M107519 | IMPORT | PAID | $1208 | 7 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
