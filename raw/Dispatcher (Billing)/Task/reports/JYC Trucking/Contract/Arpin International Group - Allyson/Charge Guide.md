---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Arpin International Group - Allyson
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Arpin International Group - Allyson (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**10% auto.** Of 187 charge lines, **19 from the tariff** (auto-approvable) and **168 hand-keyed**. *(observed)*

- Charge sets: 36 (PAID:27, INVOICED:6, DRAFT:2, UNAPPROVED:1).
- Load types: IMPORT:28, EXPORT:8.
- Biggest manual charge: **Chassis (34)** — #1 conversion target.
- Approved by: Andrea:35, Christian:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $45 (34x), $50 (2x) | days the chassis is held | mixed (2t/34m) |
| Base Price | flat per move — $450 (34x), $11.71 (1x) | the lane (load itself) | mixed (2t/34m) |
| PrePull | flat — $130 (19x), $150 (2x) | pull date vs last-free-day | mixed (2t/19m) |
| YARD STORAGE | per day — $45 (18x), $50 (2x) | yard dwell days | mixed (2t/18m) |
| EMPTY LOAD STORAGE | per day — $45 (13x), $50 (2x) | empty dwell days | mixed (2t/13m) |
| PORT DETENTION | per hour after free — $70 (18x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| pier_pass | flat pass-through — $78 (9x), $39 (1x) | pass-through (port receipt) | mixed (3t/7m) |
| CLEAN TRUCK FEE | confirm — $20 (9x), $10 (1x) | confirm source | mixed (3t/7m) |
| PIER PASS ADMIN FEE | flat pass-through — $25 (6x), $50 (4x) | pass-through (port receipt) | mixed (3t/7m) |
| DELIVERY DETENTION | per hour after free — $70 (4x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| Hazmat | confirm — $150 (3x) | confirm source | manual |
| WEEKEND PICK-UP | confirm — $100 (1x) | confirm source | manual |
| WEEKEND DELIVERY | confirm — $100 (1x) | confirm source | manual |
| SPLIT CHASSIS | per day — $75 (1x) | days the chassis is held | manual |
| drop_charge | confirm — $200 (1x) | confirm source | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **Hazmat** — (confirm meaning with carrier). *(inferred)*
- **WEEKEND PICK-UP** — (confirm meaning with carrier). *(inferred)*
- **WEEKEND DELIVERY** — (confirm meaning with carrier). *(inferred)*
- **SPLIT CHASSIS** — Chassis usage. *(inferred)*
- **drop_charge** — (confirm meaning with carrier). *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (10%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 36 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 36/36 sets · $45(34x), $50(2x) · 2t/34m

> Loads: JYCT_E107401, JYCT_E107617, JYCT_E107781, JYCT_E107900, JYCT_E107957, JYCT_E107995, JYCT_E108607, JYCT_E108651, JYCT_M107500, JYCT_M107506, JYCT_M107518, JYCT_M107632, JYCT_M107719, JYCT_M107720, JYCT_M107726, JYCT_M107822, JYCT_M107823, JYCT_M107844, JYCT_M107905, JYCT_M107985, JYCT_M107986, JYCT_M108032, JYCT_M108095, JYCT_M108183, JYCT_M108208, JYCT_M108256, JYCT_M108294, JYCT_M108295, JYCT_M108333, JYCT_M108344, JYCT_M108432, JYCT_M108460, JYCT_M108461, JYCT_M108512, JYCT_M108522, JYCT_M108685

**Base Price** — 36/36 sets · $450(34x), $11.71(1x), $11.32(1x) · 2t/34m

> Loads: JYCT_E107401, JYCT_E107617, JYCT_E107781, JYCT_E107900, JYCT_E107957, JYCT_E107995, JYCT_E108607, JYCT_E108651, JYCT_M107500, JYCT_M107506, JYCT_M107518, JYCT_M107632, JYCT_M107719, JYCT_M107720, JYCT_M107726, JYCT_M107822, JYCT_M107823, JYCT_M107844, JYCT_M107905, JYCT_M107985, JYCT_M107986, JYCT_M108032, JYCT_M108095, JYCT_M108183, JYCT_M108208, JYCT_M108256, JYCT_M108294, JYCT_M108295, JYCT_M108333, JYCT_M108344, JYCT_M108432, JYCT_M108460, JYCT_M108461, JYCT_M108512, JYCT_M108522, JYCT_M108685

**PrePull** — 21/36 sets · $130(19x), $150(2x) · 2t/19m

> Loads: JYCT_E107401, JYCT_E107617, JYCT_E107900, JYCT_E107957, JYCT_M107500, JYCT_M107518, JYCT_M107719, JYCT_M107720, JYCT_M107985, JYCT_M107986, JYCT_M108032, JYCT_M108208, JYCT_M108256, JYCT_M108333, JYCT_M108344, JYCT_M108432, JYCT_M108460, JYCT_M108461, JYCT_M108512, JYCT_M108522, JYCT_M108685

**YARD STORAGE** — 20/36 sets · $45(18x), $50(2x) · 2t/18m

> Loads: JYCT_E107401, JYCT_E107781, JYCT_E107900, JYCT_M107500, JYCT_M107518, JYCT_M107719, JYCT_M107720, JYCT_M107985, JYCT_M107986, JYCT_M108032, JYCT_M108208, JYCT_M108256, JYCT_M108333, JYCT_M108344, JYCT_M108432, JYCT_M108460, JYCT_M108461, JYCT_M108512, JYCT_M108522, JYCT_M108685

**EMPTY LOAD STORAGE** — 15/36 sets · $45(13x), $50(2x) · 2t/13m

> Loads: JYCT_E107401, JYCT_E107617, JYCT_E107900, JYCT_E107957, JYCT_M107632, JYCT_M107823, JYCT_M107905, JYCT_M108183, JYCT_M108294, JYCT_M108295, JYCT_M108344, JYCT_M108460, JYCT_M108461, JYCT_M108512, JYCT_M108685

**PORT DETENTION** — 12/36 sets · $70(18x) · manual

> Loads: JYCT_E107617, JYCT_E107781, JYCT_E108607, JYCT_E108651, JYCT_M107518, JYCT_M107719, JYCT_M107905, JYCT_M107986, JYCT_M108460, JYCT_M108461, JYCT_M108512, JYCT_M108522

**pier_pass** — 10/36 sets · $78(9x), $39(1x) · 3t/7m

> Loads: JYCT_E107401, JYCT_E107617, JYCT_E107781, JYCT_E107900, JYCT_E107957, JYCT_E107995, JYCT_E108607, JYCT_E108651, JYCT_M108344, JYCT_M108685

**CLEAN TRUCK FEE** — 10/36 sets · $20(9x), $10(1x) · 3t/7m

> Loads: JYCT_E107401, JYCT_E107617, JYCT_E107781, JYCT_E107900, JYCT_E107957, JYCT_E107995, JYCT_E108607, JYCT_E108651, JYCT_M108344, JYCT_M108685

**PIER PASS ADMIN FEE** — 10/36 sets · $25(6x), $50(4x) · 3t/7m

> Loads: JYCT_E107401, JYCT_E107617, JYCT_E107781, JYCT_E107900, JYCT_E107957, JYCT_E107995, JYCT_E108607, JYCT_E108651, JYCT_M108344, JYCT_M108685

**DELIVERY DETENTION** — 4/36 sets · $70(4x) · manual

> Loads: JYCT_E107617, JYCT_E107781, JYCT_M107506, JYCT_M107985

**Hazmat** — 3/36 sets · $150(3x) · manual

> Loads: JYCT_E107401, JYCT_M107986, JYCT_M108183

**WEEKEND PICK-UP** — 1/36 sets · $100(1x) · manual

> Loads: JYCT_M108522

**WEEKEND DELIVERY** — 1/36 sets · $100(1x) · manual

> Loads: JYCT_M108512

**SPLIT CHASSIS** — 1/36 sets · $75(1x) · manual

> Loads: JYCT_M108295

**drop_charge** — 1/36 sets · $200(1x) · manual

> Loads: JYCT_M107985

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E107401 | EXPORT | PAID | $1029 | 9 | Andrea | no |
| JYCT_E107617 | EXPORT | PAID | $971.2 | 9 | Andrea | no |
| JYCT_E107781 | EXPORT | PAID | $887 | 8 | Andrea | no |
| JYCT_E107900 | EXPORT | UNAPPROVED | $848 | 8 | Andrea | yes |
| JYCT_E107957 | EXPORT | PAID | $883 | 7 | Christian | no |
| JYCT_E107995 | EXPORT | PAID | $618 | 5 | Andrea | no |
| JYCT_E108607 | EXPORT | INVOICED | $643 | 7 | Andrea | no |
| JYCT_E108651 | EXPORT | INVOICED | $625 | 7 | Andrea | no |
| JYCT_M107500 | IMPORT | PAID | $940 | 4 | Andrea | no |
| JYCT_M107506 | IMPORT | PAID | $605.6 | 3 | Andrea | no |
| JYCT_M107518 | IMPORT | PAID | $788.7 | 5 | Andrea | no |
| JYCT_M107632 | IMPORT | PAID | $540 | 3 | Andrea | no |
| JYCT_M107719 | IMPORT | PAID | $800.6 | 5 | Andrea | no |
| JYCT_M107720 | IMPORT | PAID | $760 | 4 | Andrea | no |
| JYCT_M107726 | IMPORT | PAID | $495 | 2 | Andrea | no |
| JYCT_M107822 | IMPORT | PAID | $495 | 2 | Andrea | no |
| JYCT_M107823 | IMPORT | PAID | $540 | 3 | Andrea | no |
| JYCT_M107844 | IMPORT | PAID | $495 | 2 | Andrea | no |
| JYCT_M107905 | IMPORT | PAID | $570.1 | 4 | Andrea | no |
| JYCT_M107985 | IMPORT | PAID | $932.5 | 6 | Andrea | no |
| JYCT_M107986 | IMPORT | PAID | $1039.5 | 6 | Andrea | no |
| JYCT_M108032 | IMPORT | PAID | $670 | 4 | Andrea | no |
| JYCT_M108095 | IMPORT | PAID | $495 | 2 | Andrea | no |
| JYCT_M108183 | IMPORT | PAID | $780 | 4 | Andrea | no |
| JYCT_M108208 | IMPORT | PAID | $670 | 4 | Andrea | no |
| JYCT_M108256 | IMPORT | PAID | $670 | 4 | Andrea | no |
| JYCT_M108294 | IMPORT | PAID | $540 | 3 | Andrea | no |
| JYCT_M108295 | IMPORT | PAID | $660 | 4 | Andrea | no |
| JYCT_M108333 | IMPORT | PAID | $760 | 4 | Andrea | no |
| JYCT_M108344 | IMPORT | DRAFT | $863 | 8 | Andrea | yes |
| JYCT_M108432 | IMPORT | PAID | $670 | 4 | Andrea | no |
| JYCT_M108460 | IMPORT | INVOICED | $760 | 7 | Andrea | no |
| JYCT_M108461 | IMPORT | INVOICED | $772.6 | 7 | Andrea | no |
| JYCT_M108512 | IMPORT | INVOICED | $950 | 8 | Andrea | no |
| JYCT_M108522 | IMPORT | INVOICED | $989.5 | 7 | Andrea | no |
| JYCT_M108685 | IMPORT | DRAFT | $948 | 8 | Andrea | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
