---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Wheelhouse Logistics
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Wheelhouse Logistics (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 80 charge lines, **0 from the tariff** (auto-approvable) and **80 hand-keyed**. *(observed)*

- Charge sets: 8 (PAID:8).
- Load types: IMPORT:8.
- Biggest manual charge: **Base Price (8)** — #1 conversion target.
- Approved by: Juan Andres:8 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $550 (6x), $2565 (2x) | the lane (load itself) | manual |
| CLEAN TRUCK FEE | confirm — $20 (8x) | confirm source | manual |
| pier_pass | flat pass-through — $78 (8x) | pass-through (port receipt) | manual |
| PIER PASS ADMIN FEE | flat pass-through — $20 (7x), $50 (1x) | pass-through (port receipt) | manual |
| Chassis | per day — $50 (8x) | days the chassis is held | manual |
| PrePull | flat — $125 (7x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $50 (7x) | yard dwell days | manual |
| DELIVERY DETENTION | per hour after free — $70 (6x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| EMPTY LOAD STORAGE | per day — $50 (4x) | empty dwell days | manual |
| PORT DETENTION | per hour after free — $70 (3x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| other | unknown — $375 (3x), $30 (3x) | unknown | manual |
| WEIGHT REDISTRIBUTION | confirm — $550 (2x) | confirm source | manual |
| scale_load | flat — $15 (2x) | scale ticket | manual |
| HEAVY CONTAINER FEE | flat — $150 (2x) | overweight flag | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **other** — UNDEFINED line — must be identified before trust. *(inferred)*
- **WEIGHT REDISTRIBUTION** — (confirm meaning with carrier). *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*
- **HEAVY CONTAINER FEE** — Overweight container surcharge. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 8 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Base Price** — 8/8 sets · $550(6x), $2565(2x) · manual

> Loads: JYCT_M107901, JYCT_M107922, JYCT_M107923, JYCT_M107940, JYCT_M107942, JYCT_M107944, JYCT_M108330, JYCT_M108331

**CLEAN TRUCK FEE** — 8/8 sets · $20(8x) · manual

> Loads: JYCT_M107901, JYCT_M107922, JYCT_M107923, JYCT_M107940, JYCT_M107942, JYCT_M107944, JYCT_M108330, JYCT_M108331

**pier_pass** — 8/8 sets · $78(8x) · manual

> Loads: JYCT_M107901, JYCT_M107922, JYCT_M107923, JYCT_M107940, JYCT_M107942, JYCT_M107944, JYCT_M108330, JYCT_M108331

**PIER PASS ADMIN FEE** — 8/8 sets · $20(7x), $50(1x) · manual

> Loads: JYCT_M107901, JYCT_M107922, JYCT_M107923, JYCT_M107940, JYCT_M107942, JYCT_M107944, JYCT_M108330, JYCT_M108331

**Chassis** — 8/8 sets · $50(8x) · manual

> Loads: JYCT_M107901, JYCT_M107922, JYCT_M107923, JYCT_M107940, JYCT_M107942, JYCT_M107944, JYCT_M108330, JYCT_M108331

**PrePull** — 7/8 sets · $125(7x) · manual

> Loads: JYCT_M107901, JYCT_M107923, JYCT_M107940, JYCT_M107942, JYCT_M107944, JYCT_M108330, JYCT_M108331

**YARD STORAGE** — 7/8 sets · $50(7x) · manual

> Loads: JYCT_M107901, JYCT_M107923, JYCT_M107940, JYCT_M107942, JYCT_M107944, JYCT_M108330, JYCT_M108331

**DELIVERY DETENTION** — 6/8 sets · $70(6x) · manual

> Loads: JYCT_M107901, JYCT_M107922, JYCT_M107923, JYCT_M107944, JYCT_M108330, JYCT_M108331

**EMPTY LOAD STORAGE** — 4/8 sets · $50(4x) · manual

> Loads: JYCT_M107922, JYCT_M107944, JYCT_M108330, JYCT_M108331

**PORT DETENTION** — 3/8 sets · $70(3x) · manual

> Loads: JYCT_M107923, JYCT_M107942, JYCT_M108330

**other** — 3/8 sets · $375(3x), $30(3x), $62(1x) · manual

> Loads: JYCT_M107940, JYCT_M107942, JYCT_M107944

**WEIGHT REDISTRIBUTION** — 2/8 sets · $550(2x) · manual

> Loads: JYCT_M108330, JYCT_M108331

**scale_load** — 2/8 sets · $15(2x) · manual

> Loads: JYCT_M108330, JYCT_M108331

**HEAVY CONTAINER FEE** — 2/8 sets · $150(2x) · manual

> Loads: JYCT_M108330, JYCT_M108331

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107901 | IMPORT | PAID | $1086.1 | 8 | Juan Andres | no |
| JYCT_M107922 | IMPORT | PAID | $808.6 | 7 | Juan Andres | no |
| JYCT_M107923 | IMPORT | PAID | $1106.4 | 9 | Juan Andres | no |
| JYCT_M107940 | IMPORT | PAID | $1398 | 9 | Juan Andres | no |
| JYCT_M107942 | IMPORT | PAID | $1444.2 | 10 | Juan Andres | no |
| JYCT_M107944 | IMPORT | PAID | $1639.1 | 12 | Juan Andres | no |
| JYCT_M108330 | IMPORT | PAID | $4107.4 | 13 | Juan Andres | no |
| JYCT_M108331 | IMPORT | PAID | $4198 | 12 | Juan Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
