---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Quick Pick Logistics LLC-*B
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Quick Pick Logistics LLC-*B (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 56 charge lines, **0 from the tariff** (auto-approvable) and **56 hand-keyed**. *(observed)*

- Charge sets: 9 (PAID:9).
- Load types: IMPORT:9.
- Biggest manual charge: **Base Price (9)** — #1 conversion target.
- Approved by: Andres:9 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $4576 (6x), $470 (1x) | the lane (load itself) | manual |
| EMPTY LOAD STORAGE | per day — $45 (7x), $22.5 (1x) | empty dwell days | manual |
| tri_axle | confirm — $100 (7x) | confirm source | manual |
| DELIVERY DETENTION | per hour after free — $70 (5x), $85 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| PrePull | flat — $125 (5x), $25 (1x) | pull date vs last-free-day | manual |
| scale_load | flat — $15 (6x) | scale ticket | manual |
| YARD STORAGE | per day — $45 (6x) | yard dwell days | manual |
| drop_charge | confirm — $200 (1x), $157 (1x) | confirm source | manual |
| Chassis | per day — $45 (2x) | days the chassis is held | manual |
| bonded_cargo_charge | flat — $100 (1x) | bonded move flag | manual |
| other | unknown — $300 (1x) | unknown | manual |
| PORT DETENTION | per hour after free — $49.35 (1x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **tri_axle** — (confirm meaning with carrier). *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **bonded_cargo_charge** — Bonded-cargo handling. *(inferred)*
- **other** — UNDEFINED line — must be identified before trust. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 9 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Base Price** — 9/9 sets · $4576(6x), $470(1x), $315(1x) · manual

> Loads: JYCT_M107644, JYCT_M107920, JYCT_M107924, JYCT_M107925, JYCT_M107926, JYCT_M107927, JYCT_M107972, JYCT_M107973, JYCT_M108430

**EMPTY LOAD STORAGE** — 8/9 sets · $45(7x), $22.5(1x) · manual

> Loads: JYCT_M107644, JYCT_M107924, JYCT_M107925, JYCT_M107926, JYCT_M107927, JYCT_M107972, JYCT_M107973, JYCT_M108430

**tri_axle** — 7/9 sets · $100(7x) · manual

> Loads: JYCT_M107644, JYCT_M107924, JYCT_M107925, JYCT_M107926, JYCT_M107927, JYCT_M107972, JYCT_M107973

**DELIVERY DETENTION** — 7/9 sets · $70(5x), $85(1x), $17.5(1x) · manual

> Loads: JYCT_M107644, JYCT_M107920, JYCT_M107924, JYCT_M107925, JYCT_M107927, JYCT_M107972, JYCT_M107973

**PrePull** — 6/9 sets · $125(5x), $25(1x) · manual

> Loads: JYCT_M107924, JYCT_M107925, JYCT_M107926, JYCT_M107927, JYCT_M107972, JYCT_M107973

**scale_load** — 6/9 sets · $15(6x) · manual

> Loads: JYCT_M107924, JYCT_M107925, JYCT_M107926, JYCT_M107927, JYCT_M107972, JYCT_M107973

**YARD STORAGE** — 6/9 sets · $45(6x) · manual

> Loads: JYCT_M107924, JYCT_M107925, JYCT_M107926, JYCT_M107927, JYCT_M107972, JYCT_M107973

**drop_charge** — 2/9 sets · $200(1x), $157(1x) · manual

> Loads: JYCT_M107920, JYCT_M108430

**Chassis** — 2/9 sets · $45(2x) · manual

> Loads: JYCT_M107920, JYCT_M108430

**bonded_cargo_charge** — 1/9 sets · $100(1x) · manual

> Loads: JYCT_M108430

**other** — 1/9 sets · $300(1x) · manual

> Loads: JYCT_M107924

**PORT DETENTION** — 1/9 sets · $49.35(1x) · manual

> Loads: JYCT_M107644

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107644 | IMPORT | PAID | $1704.35 | 5 | Andres | no |
| JYCT_M107920 | IMPORT | PAID | $439.85 | 4 | Andres | no |
| JYCT_M107924 | IMPORT | PAID | $5757.6 | 8 | Andres | no |
| JYCT_M107925 | IMPORT | PAID | $5889.1 | 7 | Andres | no |
| JYCT_M107926 | IMPORT | PAID | $5931 | 6 | Andres | no |
| JYCT_M107927 | IMPORT | PAID | $5889.1 | 7 | Andres | no |
| JYCT_M107972 | IMPORT | PAID | $5656 | 7 | Andres | no |
| JYCT_M107973 | IMPORT | PAID | $5466 | 7 | Andres | no |
| JYCT_M108430 | IMPORT | PAID | $1040 | 5 | Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
