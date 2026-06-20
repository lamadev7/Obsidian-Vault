---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: CARGO LINK USA LLC - SOPHIE
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — CARGO LINK USA LLC - SOPHIE (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**4% auto.** Of 83 charge lines, **3 from the tariff** (auto-approvable) and **80 hand-keyed**. *(observed)*

- Charge sets: 22 (PAID:19, INVOICED:2, DRAFT:1).
- Load types: EXPORT:15, IMPORT:7.
- Biggest manual charge: **bonded_cargo_charge (12)** — #1 conversion target.
- Approved by: Andres:21, Miguel:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| bonded_cargo_charge | flat — $140 (9x), $250 (3x) | bonded move flag | manual |
| BONDED 7512 FORM | flat — $250 (10x) | bonded move flag | manual |
| Chassis | per day — $45 (9x) | days the chassis is held | manual |
| Base Price | flat per move — $3100 (6x), $3744 (1x) | the lane (load itself) | manual |
| PrePull | flat — $150 (7x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $40 (7x) | yard dwell days | manual |
| EMPTY LOAD STORAGE | per day — $40 (6x) | empty dwell days | manual |
| scale_load | flat — $15 (5x), $14.75 (1x) | scale ticket | manual |
| DELIVERY DETENTION | per hour after free — $70 (5x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| PORT DETENTION | per hour after free — $70 (4x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| reefer | confirm — $150 (1x), $175 (1x) | confirm source | manual |
| drop_charge | confirm — $150 (1x), $200 (1x) | confirm source | manual |
| pier_pass | flat pass-through — $78 (1x) | pass-through (port receipt) | tariff |
| CLEAN TRUCK FEE | confirm — $20 (1x) | confirm source | tariff |
| PIER PASS ADMIN FEE | flat pass-through — $50 (1x) | pass-through (port receipt) | tariff |
| other | unknown — $280 (1x) | unknown | manual |

## Definitions & deviations

- **bonded_cargo_charge** — Bonded-cargo handling. *(inferred)*
- **BONDED 7512 FORM** — Bonded-cargo handling. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **reefer** — (confirm meaning with carrier). *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **other** — UNDEFINED line — must be identified before trust. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (4%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 22 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**bonded_cargo_charge** — 12/22 sets · $140(9x), $250(3x) · manual

> Loads: JYCT_E107558, JYCT_E108101, JYCT_E108117, JYCT_E108159, JYCT_E108611, JYCT_M107857, JYCT_M107865, JYCT_M107889, JYCT_M107890, JYCT_M107935, JYCT_M107936, JYCT_M108701

**BONDED 7512 FORM** — 10/22 sets · $250(10x) · manual

> Loads: JYCT_E108387, JYCT_E108388, JYCT_E108389, JYCT_E108390, JYCT_E108393, JYCT_E108394, JYCT_E108400, JYCT_E108401, JYCT_E108402, JYCT_E108595

**Chassis** — 9/22 sets · $45(9x) · manual

> Loads: JYCT_E107558, JYCT_E108117, JYCT_M107857, JYCT_M107865, JYCT_M107889, JYCT_M107890, JYCT_M107935, JYCT_M107936, JYCT_M108701

**Base Price** — 9/22 sets · $3100(6x), $3744(1x), $520(1x) · manual

> Loads: JYCT_E107558, JYCT_E108117, JYCT_M107857, JYCT_M107865, JYCT_M107889, JYCT_M107890, JYCT_M107935, JYCT_M107936, JYCT_M108701

**PrePull** — 7/22 sets · $150(7x) · manual

> Loads: JYCT_M107857, JYCT_M107865, JYCT_M107889, JYCT_M107890, JYCT_M107935, JYCT_M107936, JYCT_M108701

**YARD STORAGE** — 7/22 sets · $40(7x) · manual

> Loads: JYCT_M107857, JYCT_M107865, JYCT_M107889, JYCT_M107890, JYCT_M107935, JYCT_M107936, JYCT_M108701

**EMPTY LOAD STORAGE** — 6/22 sets · $40(6x) · manual

> Loads: JYCT_M107857, JYCT_M107865, JYCT_M107889, JYCT_M107890, JYCT_M107935, JYCT_M108701

**scale_load** — 6/22 sets · $15(5x), $14.75(1x) · manual

> Loads: JYCT_M107857, JYCT_M107889, JYCT_M107890, JYCT_M107935, JYCT_M107936, JYCT_M108701

**DELIVERY DETENTION** — 5/22 sets · $70(5x) · manual

> Loads: JYCT_M107857, JYCT_M107865, JYCT_M107889, JYCT_M107890, JYCT_M107936

**PORT DETENTION** — 3/22 sets · $70(4x) · manual

> Loads: JYCT_E107558, JYCT_E108117, JYCT_M107865

**reefer** — 2/22 sets · $150(1x), $175(1x) · manual

> Loads: JYCT_E107558, JYCT_E108117

**drop_charge** — 2/22 sets · $150(1x), $200(1x) · manual

> Loads: JYCT_E107558, JYCT_E108117

**pier_pass** — 1/22 sets · $78(1x) · tariff

> Loads: JYCT_M108701

**CLEAN TRUCK FEE** — 1/22 sets · $20(1x) · tariff

> Loads: JYCT_M108701

**PIER PASS ADMIN FEE** — 1/22 sets · $50(1x) · tariff

> Loads: JYCT_M108701

**other** — 1/22 sets · $280(1x) · manual

> Loads: JYCT_M107889

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E107558 | EXPORT | PAID | $1318.5 | 6 | Miguel | no |
| JYCT_E108101 | EXPORT | PAID | $250 | 1 | Andres | no |
| JYCT_E108117 | EXPORT | PAID | $1548.1 | 6 | Andres | no |
| JYCT_E108159 | EXPORT | PAID | $250 | 1 | Andres | no |
| JYCT_E108387 | EXPORT | PAID | $250 | 1 | Andres | no |
| JYCT_E108388 | EXPORT | PAID | $250 | 1 | Andres | no |
| JYCT_E108389 | EXPORT | PAID | $250 | 1 | Andres | no |
| JYCT_E108390 | EXPORT | PAID | $250 | 1 | Andres | no |
| JYCT_E108393 | EXPORT | PAID | $250 | 1 | Andres | no |
| JYCT_E108394 | EXPORT | PAID | $250 | 1 | Andres | no |
| JYCT_E108400 | EXPORT | PAID | $250 | 1 | Andres | no |
| JYCT_E108401 | EXPORT | PAID | $250 | 1 | Andres | no |
| JYCT_E108402 | EXPORT | PAID | $250 | 1 | Andres | no |
| JYCT_E108595 | EXPORT | INVOICED | $250 | 1 | Andres | no |
| JYCT_E108611 | EXPORT | INVOICED | $250 | 1 | Andres | no |
| JYCT_M107857 | IMPORT | PAID | $3930 | 8 | Andres | no |
| JYCT_M107865 | IMPORT | PAID | $3903.1 | 9 | Andres | no |
| JYCT_M107889 | IMPORT | PAID | $4037.5 | 9 | Andres | no |
| JYCT_M107890 | IMPORT | PAID | $3775.6 | 8 | Andres | no |
| JYCT_M107935 | IMPORT | PAID | $3835 | 7 | Andres | no |
| JYCT_M107936 | IMPORT | PAID | $3981.7 | 7 | Andres | no |
| JYCT_M108701 | IMPORT | DRAFT | $4242 | 10 | Andres | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
