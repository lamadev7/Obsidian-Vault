---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Island Way Sorbet - Jared
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Island Way Sorbet - Jared (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 79 charge lines, **0 from the tariff** (auto-approvable) and **79 hand-keyed**. *(observed)*

- Charge sets: 10 (INVOICED:7, PAID:3).
- Load types: IMPORT:10.
- Biggest manual charge: **PORT DETENTION (14)** — #1 conversion target.
- Approved by: Andrea:10 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $520 (9x), $670 (1x) | the lane (load itself) | manual |
| Chassis | per day — $50 (10x) | days the chassis is held | manual |
| reefer | confirm — $150 (10x) | confirm source | manual |
| PrePull | flat — $150 (9x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $45 (9x) | yard dwell days | manual |
| drop_charge | confirm — $250 (9x) | confirm source | manual |
| PORT DETENTION | per hour after free — $70 (14x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| EMPTY LOAD STORAGE | per day — $45 (7x) | empty dwell days | manual |
| WEEKEND DELIVERY | confirm — $100 (1x) | confirm source | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **reefer** — (confirm meaning with carrier). *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **WEEKEND DELIVERY** — (confirm meaning with carrier). *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 10 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Base Price** — 10/10 sets · $520(9x), $670(1x) · manual

> Loads: JYCT_M108318, JYCT_M108319, JYCT_M108320, JYCT_M108378, JYCT_M108514, JYCT_M108515, JYCT_M108566, JYCT_M108567, JYCT_M108647, JYCT_M108648

**Chassis** — 10/10 sets · $50(10x) · manual

> Loads: JYCT_M108318, JYCT_M108319, JYCT_M108320, JYCT_M108378, JYCT_M108514, JYCT_M108515, JYCT_M108566, JYCT_M108567, JYCT_M108647, JYCT_M108648

**reefer** — 10/10 sets · $150(10x) · manual

> Loads: JYCT_M108318, JYCT_M108319, JYCT_M108320, JYCT_M108378, JYCT_M108514, JYCT_M108515, JYCT_M108566, JYCT_M108567, JYCT_M108647, JYCT_M108648

**PrePull** — 9/10 sets · $150(9x) · manual

> Loads: JYCT_M108318, JYCT_M108319, JYCT_M108320, JYCT_M108514, JYCT_M108515, JYCT_M108566, JYCT_M108567, JYCT_M108647, JYCT_M108648

**YARD STORAGE** — 9/10 sets · $45(9x) · manual

> Loads: JYCT_M108318, JYCT_M108319, JYCT_M108320, JYCT_M108514, JYCT_M108515, JYCT_M108566, JYCT_M108567, JYCT_M108647, JYCT_M108648

**drop_charge** — 9/10 sets · $250(9x) · manual

> Loads: JYCT_M108319, JYCT_M108320, JYCT_M108378, JYCT_M108514, JYCT_M108515, JYCT_M108566, JYCT_M108567, JYCT_M108647, JYCT_M108648

**PORT DETENTION** — 8/10 sets · $70(14x) · manual

> Loads: JYCT_M108318, JYCT_M108320, JYCT_M108514, JYCT_M108515, JYCT_M108566, JYCT_M108567, JYCT_M108647, JYCT_M108648

**EMPTY LOAD STORAGE** — 7/10 sets · $45(7x) · manual

> Loads: JYCT_M108318, JYCT_M108378, JYCT_M108514, JYCT_M108515, JYCT_M108566, JYCT_M108647, JYCT_M108648

**WEEKEND DELIVERY** — 1/10 sets · $100(1x) · manual

> Loads: JYCT_M108566

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M108318 | IMPORT | INVOICED | $1471.6 | 7 | Andrea | no |
| JYCT_M108319 | IMPORT | PAID | $1360 | 6 | Andrea | no |
| JYCT_M108320 | IMPORT | PAID | $1386.6 | 7 | Andrea | no |
| JYCT_M108378 | IMPORT | PAID | $1115 | 5 | Andrea | no |
| JYCT_M108514 | IMPORT | INVOICED | $1455 | 9 | Andrea | no |
| JYCT_M108515 | IMPORT | INVOICED | $1581.7 | 9 | Andrea | no |
| JYCT_M108566 | IMPORT | INVOICED | $1734.5 | 10 | Andrea | no |
| JYCT_M108567 | IMPORT | INVOICED | $1459.4 | 8 | Andrea | no |
| JYCT_M108647 | IMPORT | INVOICED | $1648 | 9 | Andrea | no |
| JYCT_M108648 | IMPORT | INVOICED | $1672.9 | 9 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
