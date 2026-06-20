---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: LOGIFLEX CARGO - John
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — LOGIFLEX CARGO - John (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 114 charge lines, **0 from the tariff** (auto-approvable) and **114 hand-keyed**. *(observed)*

- Charge sets: 23 (PAID:22, INVOICED:1).
- Load types: IMPORT:23.
- Biggest manual charge: **Base Price (24)** — #1 conversion target.
- Approved by: Andrea:23 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $35 (23x) | days the chassis is held | manual |
| Base Price | flat per move — $565 (23x), $350 (1x) | the lane (load itself) | manual |
| YARD STORAGE | per day — $35 (21x) | yard dwell days | manual |
| PrePull | flat — $150 (11x), $125 (2x) | pull date vs last-free-day | manual |
| DELIVERY DETENTION | per hour after free — $70 (8x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| EMPTY LOAD STORAGE | per day — $35 (6x) | empty dwell days | manual |
| drop_charge | confirm — $150 (6x) | confirm source | manual |
| PORT DETENTION | per hour after free — $70 (9x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| stop_off | flat per stop — $100 (3x) | count of extra stops on the load | manual |
| dry_run | flat — $400 (1x) | trouble-call evidence | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **stop_off** — Extra stop-off. *(inferred)*
- **dry_run** — Dry run / trouble call. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff not wired** — every line hand-keyed. The whole menu is the conversion target. *(observed)*

## Validate against history

- All 23 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 23/23 sets · $35(23x) · manual

> Loads: JYCT_M107525, JYCT_M107613, JYCT_M107614, JYCT_M107673, JYCT_M107848, JYCT_M107849, JYCT_M107850, JYCT_M108004, JYCT_M108005, JYCT_M108006, JYCT_M108007, JYCT_M108217, JYCT_M108218, JYCT_M108326, JYCT_M108327, JYCT_M108328, JYCT_M108417, JYCT_M108418, JYCT_M108419, JYCT_M108524, JYCT_M108581, JYCT_M108656, JYCT_M108715

**Base Price** — 23/23 sets · $565(23x), $350(1x) · manual

> Loads: JYCT_M107525, JYCT_M107613, JYCT_M107614, JYCT_M107673, JYCT_M107848, JYCT_M107849, JYCT_M107850, JYCT_M108004, JYCT_M108005, JYCT_M108006, JYCT_M108007, JYCT_M108217, JYCT_M108218, JYCT_M108326, JYCT_M108327, JYCT_M108328, JYCT_M108417, JYCT_M108418, JYCT_M108419, JYCT_M108524, JYCT_M108581, JYCT_M108656, JYCT_M108715

**YARD STORAGE** — 21/23 sets · $35(21x) · manual

> Loads: JYCT_M107525, JYCT_M107613, JYCT_M107614, JYCT_M107673, JYCT_M107848, JYCT_M107849, JYCT_M107850, JYCT_M108004, JYCT_M108005, JYCT_M108007, JYCT_M108217, JYCT_M108218, JYCT_M108326, JYCT_M108327, JYCT_M108328, JYCT_M108417, JYCT_M108418, JYCT_M108419, JYCT_M108524, JYCT_M108656, JYCT_M108715

**PrePull** — 13/23 sets · $150(11x), $125(2x) · manual

> Loads: JYCT_M108004, JYCT_M108217, JYCT_M108218, JYCT_M108326, JYCT_M108327, JYCT_M108328, JYCT_M108417, JYCT_M108418, JYCT_M108419, JYCT_M108524, JYCT_M108581, JYCT_M108656, JYCT_M108715

**DELIVERY DETENTION** — 7/23 sets · $70(8x) · manual

> Loads: JYCT_M107673, JYCT_M107850, JYCT_M108006, JYCT_M108328, JYCT_M108417, JYCT_M108419, JYCT_M108581

**EMPTY LOAD STORAGE** — 6/23 sets · $35(6x) · manual

> Loads: JYCT_M107525, JYCT_M108006, JYCT_M108007, JYCT_M108217, JYCT_M108327, JYCT_M108715

**drop_charge** — 6/23 sets · $150(6x) · manual

> Loads: JYCT_M108005, JYCT_M108007, JYCT_M108217, JYCT_M108218, JYCT_M108326, JYCT_M108327

**PORT DETENTION** — 5/23 sets · $70(9x) · manual

> Loads: JYCT_M108326, JYCT_M108328, JYCT_M108581, JYCT_M108656, JYCT_M108715

**stop_off** — 3/23 sets · $100(3x) · manual

> Loads: JYCT_M108007, JYCT_M108328, JYCT_M108581

**dry_run** — 1/23 sets · $400(1x) · manual

> Loads: JYCT_M108004

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107525 | IMPORT | PAID | $845 | 4 | Andrea | no |
| JYCT_M107613 | IMPORT | PAID | $1300 | 3 | Andrea | no |
| JYCT_M107614 | IMPORT | PAID | $985 | 3 | Andrea | no |
| JYCT_M107673 | IMPORT | PAID | $915 | 4 | Andrea | no |
| JYCT_M107848 | IMPORT | PAID | $950 | 3 | Andrea | no |
| JYCT_M107849 | IMPORT | PAID | $1020 | 3 | Andrea | no |
| JYCT_M107850 | IMPORT | PAID | $658.1 | 4 | Andrea | no |
| JYCT_M108004 | IMPORT | PAID | $600 | 5 | Andrea | no |
| JYCT_M108005 | IMPORT | PAID | $1275 | 5 | Andrea | no |
| JYCT_M108006 | IMPORT | PAID | $611.9 | 4 | Andrea | no |
| JYCT_M108007 | IMPORT | PAID | $920 | 6 | Andrea | no |
| JYCT_M108217 | IMPORT | PAID | $890 | 6 | Andrea | no |
| JYCT_M108218 | IMPORT | PAID | $715 | 5 | Andrea | no |
| JYCT_M108326 | IMPORT | PAID | $715 | 6 | Andrea | no |
| JYCT_M108327 | IMPORT | PAID | $820 | 6 | Andrea | no |
| JYCT_M108328 | IMPORT | PAID | $759.5 | 9 | Andrea | no |
| JYCT_M108417 | IMPORT | PAID | $652.5 | 5 | Andrea | no |
| JYCT_M108418 | IMPORT | PAID | $565 | 4 | Andrea | no |
| JYCT_M108419 | IMPORT | PAID | $600 | 5 | Andrea | no |
| JYCT_M108524 | IMPORT | PAID | $565 | 4 | Andrea | no |
| JYCT_M108581 | IMPORT | PAID | $805 | 7 | Andrea | no |
| JYCT_M108656 | IMPORT | PAID | $600 | 6 | Andrea | no |
| JYCT_M108715 | IMPORT | INVOICED | $565 | 7 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
