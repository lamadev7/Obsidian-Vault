---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: US Logistics - Noah
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — US Logistics - Noah (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 67 charge lines, **0 from the tariff** (auto-approvable) and **67 hand-keyed**. *(observed)*

- Charge sets: 8 (PAID:8).
- Load types: IMPORT:8.
- Biggest manual charge: **other (15)** — #1 conversion target.
- Approved by: Andrea:8 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| other | unknown — $206.25 (8x), $0 (4x) | unknown | manual |
| Base Price | flat per move — $450 (7x), $440 (1x) | the lane (load itself) | manual |
| Chassis | per day — $45 (7x), $50 (1x) | days the chassis is held | manual |
| transload | flat — $600 (8x) | transload job | manual |
| EMPTY LOAD STORAGE | per day — $45 (6x), $50 (1x) | empty dwell days | manual |
| PORT DETENTION | per hour after free — $70 (11x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| PrePull | flat — $125 (4x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $45 (4x) | yard dwell days | manual |
| DELIVERY DETENTION | per hour after free — $70 (2x) | consignee arrive/depart clock, cross-checked vs driver | manual |

## Definitions & deviations

- **other** — UNDEFINED line — must be identified before trust. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **transload** — Cross-dock/transload service. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*

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

**other** — 8/8 sets · $206.25(8x), $0(4x), $115.6(1x) · manual

> Loads: JYCT_M108612, JYCT_M108613, JYCT_M108614, JYCT_M108615, JYCT_M108616, JYCT_M108617, JYCT_M108618, JYCT_M108619

**Base Price** — 8/8 sets · $450(7x), $440(1x) · manual

> Loads: JYCT_M108612, JYCT_M108613, JYCT_M108614, JYCT_M108615, JYCT_M108616, JYCT_M108617, JYCT_M108618, JYCT_M108619

**Chassis** — 8/8 sets · $45(7x), $50(1x) · manual

> Loads: JYCT_M108612, JYCT_M108613, JYCT_M108614, JYCT_M108615, JYCT_M108616, JYCT_M108617, JYCT_M108618, JYCT_M108619

**transload** — 8/8 sets · $600(8x) · manual

> Loads: JYCT_M108612, JYCT_M108613, JYCT_M108614, JYCT_M108615, JYCT_M108616, JYCT_M108617, JYCT_M108618, JYCT_M108619

**EMPTY LOAD STORAGE** — 7/8 sets · $45(6x), $50(1x) · manual

> Loads: JYCT_M108612, JYCT_M108613, JYCT_M108614, JYCT_M108615, JYCT_M108616, JYCT_M108618, JYCT_M108619

**PORT DETENTION** — 7/8 sets · $70(11x) · manual

> Loads: JYCT_M108612, JYCT_M108614, JYCT_M108615, JYCT_M108616, JYCT_M108617, JYCT_M108618, JYCT_M108619

**PrePull** — 4/8 sets · $125(4x) · manual

> Loads: JYCT_M108613, JYCT_M108617, JYCT_M108618, JYCT_M108619

**YARD STORAGE** — 4/8 sets · $45(4x) · manual

> Loads: JYCT_M108613, JYCT_M108617, JYCT_M108618, JYCT_M108619

**DELIVERY DETENTION** — 2/8 sets · $70(2x) · manual

> Loads: JYCT_M108613, JYCT_M108615

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108612 | IMPORT | PAID | $1608.15 | 7 | Andrea | no |
| JYCT_M108613 | IMPORT | PAID | $1561.25 | 9 | Andrea | no |
| JYCT_M108614 | IMPORT | PAID | $1346.25 | 8 | Andrea | no |
| JYCT_M108615 | IMPORT | PAID | $1561.25 | 8 | Andrea | no |
| JYCT_M108616 | IMPORT | PAID | $1376.25 | 8 | Andrea | no |
| JYCT_M108617 | IMPORT | PAID | $1561.25 | 7 | Andrea | no |
| JYCT_M108618 | IMPORT | PAID | $1391.25 | 10 | Andrea | no |
| JYCT_M108619 | IMPORT | PAID | $1445.65 | 10 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
