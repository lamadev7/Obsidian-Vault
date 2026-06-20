---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: CALOP FREIGHT SERVICES - CRISTIAN
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — CALOP FREIGHT SERVICES - CRISTIAN (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 19 charge lines, **0 from the tariff** (auto-approvable) and **19 hand-keyed**. *(observed)*

- Charge sets: 2 (INVOICED:1, PAID:1).
- Load types: IMPORT:2.
- Biggest manual charge: **Chassis (2)** — #1 conversion target.
- Approved by: Andres:2 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $45 (2x) | days the chassis is held | manual |
| Base Price | flat per move — $875 (1x), $450 (1x) | the lane (load itself) | manual |
| DELIVERY DETENTION | per hour after free — $70 (2x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| pier_pass | flat pass-through — $78 (1x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $20 (1x) | confirm source | manual |
| PIER PASS ADMIN FEE | flat pass-through — $50 (1x) | pass-through (port receipt) | manual |
| scale_load | flat — $15.25 (1x) | scale ticket | manual |
| SCALE TICKET STOP OFF | flat per stop — $50 (1x) | count of extra stops on the load | manual |
| YARD STORAGE | per day — $45 (1x) | yard dwell days | manual |
| EMPTY LOAD STORAGE | per day — $45 (1x) | empty dwell days | manual |
| PrePull | flat — $125 (1x) | pull date vs last-free-day | manual |
| PORT DETENTION | per hour after free — $70 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| stop_off | flat per stop — $350 (1x) | count of extra stops on the load | manual |
| WEIGHT REDISTRIBUTION | confirm — $400 (1x) | confirm source | manual |
| transload | flat — $700 (1x) | transload job | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*
- **SCALE TICKET STOP OFF** — Extra stop-off. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **stop_off** — Extra stop-off. *(inferred)*
- **WEIGHT REDISTRIBUTION** — (confirm meaning with carrier). *(inferred)*
- **transload** — Cross-dock/transload service. *(inferred)*

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

> Loads: JYCT_M108405, JYCT_M108765

**Base Price** — 2/2 sets · $875(1x), $450(1x) · manual

> Loads: JYCT_M108405, JYCT_M108765

**DELIVERY DETENTION** — 2/2 sets · $70(2x) · manual

> Loads: JYCT_M108405, JYCT_M108765

**pier_pass** — 1/2 sets · $78(1x) · manual

> Loads: JYCT_M108765

**CLEAN TRUCK FEE** — 1/2 sets · $20(1x) · manual

> Loads: JYCT_M108765

**PIER PASS ADMIN FEE** — 1/2 sets · $50(1x) · manual

> Loads: JYCT_M108765

**scale_load** — 1/2 sets · $15.25(1x) · manual

> Loads: JYCT_M108765

**SCALE TICKET STOP OFF** — 1/2 sets · $50(1x) · manual

> Loads: JYCT_M108765

**YARD STORAGE** — 1/2 sets · $45(1x) · manual

> Loads: JYCT_M108765

**EMPTY LOAD STORAGE** — 1/2 sets · $45(1x) · manual

> Loads: JYCT_M108765

**PrePull** — 1/2 sets · $125(1x) · manual

> Loads: JYCT_M108765

**PORT DETENTION** — 1/2 sets · $70(2x) · manual

> Loads: JYCT_M108765

**stop_off** — 1/2 sets · $350(1x) · manual

> Loads: JYCT_M108765

**WEIGHT REDISTRIBUTION** — 1/2 sets · $400(1x) · manual

> Loads: JYCT_M108765

**transload** — 1/2 sets · $700(1x) · manual

> Loads: JYCT_M108405

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108405 | IMPORT | PAID | $1400.1 | 4 | Andres | no |
| JYCT_M108765 | IMPORT | INVOICED | $2553.5 | 15 | Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
