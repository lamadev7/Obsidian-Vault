---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Naz Apparel - Nathan
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Naz Apparel - Nathan (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 79 charge lines, **0 from the tariff** (auto-approvable) and **79 hand-keyed**. *(observed)*

- Charge sets: 15 (PAID:15).
- Load types: IMPORT:14, BILL_ONLY:1.
- Biggest manual charge: **PrePull (14)** — #1 conversion target.
- Approved by: Juan Andres:15 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| PrePull | flat — $100 (8x), $150 (5x) | pull date vs last-free-day | manual |
| Base Price | flat per move — $475 (12x), $545 (2x) | the lane (load itself) | manual |
| Chassis | per day — $35 (14x) | days the chassis is held | manual |
| drop_charge | confirm — $200 (10x), $250 (2x) | confirm source | manual |
| YARD STORAGE | per day — $35 (12x) | yard dwell days | manual |
| EMPTY LOAD STORAGE | per day — $35 (7x) | empty dwell days | manual |
| PORT DETENTION | per hour after free — $75 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| other | unknown — $321.25 (1x), $20 (1x) | unknown | manual |
| Detention | per hour after free — $85 (1x) | arrive/depart clock (name the location), cross-checked vs driver | manual |

## Definitions & deviations

- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **other** — UNDEFINED line — must be identified before trust. *(inferred)*
- **Detention** — Detention wait time. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 15 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**PrePull** — 14/15 sets · $100(8x), $150(5x), $175(1x) · manual

> Loads: JYCT_B108700, JYCT_M108023, JYCT_M108100, JYCT_M108112, JYCT_M108113, JYCT_M108114, JYCT_M108115, JYCT_M108116, JYCT_M108189, JYCT_M108191, JYCT_M108194, JYCT_M108195, JYCT_M108351, JYCT_M108527

**Base Price** — 14/15 sets · $475(12x), $545(2x) · manual

> Loads: JYCT_M108023, JYCT_M108100, JYCT_M108112, JYCT_M108113, JYCT_M108114, JYCT_M108115, JYCT_M108116, JYCT_M108189, JYCT_M108191, JYCT_M108193, JYCT_M108194, JYCT_M108195, JYCT_M108351, JYCT_M108527

**Chassis** — 14/15 sets · $35(14x) · manual

> Loads: JYCT_M108023, JYCT_M108100, JYCT_M108112, JYCT_M108113, JYCT_M108114, JYCT_M108115, JYCT_M108116, JYCT_M108189, JYCT_M108191, JYCT_M108193, JYCT_M108194, JYCT_M108195, JYCT_M108351, JYCT_M108527

**drop_charge** — 13/15 sets · $200(10x), $250(2x), $237(1x) · manual

> Loads: JYCT_M108023, JYCT_M108100, JYCT_M108112, JYCT_M108113, JYCT_M108115, JYCT_M108116, JYCT_M108189, JYCT_M108191, JYCT_M108193, JYCT_M108194, JYCT_M108195, JYCT_M108351, JYCT_M108527

**YARD STORAGE** — 12/15 sets · $35(12x) · manual

> Loads: JYCT_M108100, JYCT_M108112, JYCT_M108113, JYCT_M108114, JYCT_M108115, JYCT_M108116, JYCT_M108189, JYCT_M108191, JYCT_M108194, JYCT_M108195, JYCT_M108351, JYCT_M108527

**EMPTY LOAD STORAGE** — 7/15 sets · $35(7x) · manual

> Loads: JYCT_M108023, JYCT_M108112, JYCT_M108113, JYCT_M108114, JYCT_M108115, JYCT_M108193, JYCT_M108194

**PORT DETENTION** — 2/15 sets · $75(2x) · manual

> Loads: JYCT_M108112, JYCT_M108114

**other** — 1/15 sets · $321.25(1x), $20(1x) · manual

> Loads: JYCT_B108700

**Detention** — 1/15 sets · $85(1x) · manual

> Loads: JYCT_B108700

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_B108700 | BILL_ONLY | PAID | $567.7 | 4 | Juan Andres | no |
| JYCT_M108023 | IMPORT | PAID | $615 | 5 | Juan Andres | no |
| JYCT_M108100 | IMPORT | PAID | $685 | 5 | Juan Andres | no |
| JYCT_M108112 | IMPORT | PAID | $781 | 7 | Juan Andres | no |
| JYCT_M108113 | IMPORT | PAID | $685 | 6 | Juan Andres | no |
| JYCT_M108114 | IMPORT | PAID | $877.5 | 6 | Juan Andres | no |
| JYCT_M108115 | IMPORT | PAID | $720 | 6 | Juan Andres | no |
| JYCT_M108116 | IMPORT | PAID | $650 | 5 | Juan Andres | no |
| JYCT_M108189 | IMPORT | PAID | $685 | 5 | Juan Andres | no |
| JYCT_M108191 | IMPORT | PAID | $615 | 5 | Juan Andres | no |
| JYCT_M108193 | IMPORT | PAID | $720 | 4 | Juan Andres | no |
| JYCT_M108194 | IMPORT | PAID | $755 | 6 | Juan Andres | no |
| JYCT_M108195 | IMPORT | PAID | $650 | 5 | Juan Andres | no |
| JYCT_M108351 | IMPORT | PAID | $720 | 5 | Juan Andres | no |
| JYCT_M108527 | IMPORT | PAID | $685 | 5 | Juan Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
