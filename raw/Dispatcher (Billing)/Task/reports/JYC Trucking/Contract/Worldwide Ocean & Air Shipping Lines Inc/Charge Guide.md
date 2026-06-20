---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Worldwide Ocean & Air Shipping Lines Inc
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Worldwide Ocean & Air Shipping Lines Inc (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 17 charge lines, **0 from the tariff** (auto-approvable) and **17 hand-keyed**. *(observed)*

- Charge sets: 3 (PAID:3).
- Load types: IMPORT:2, EXPORT:1.
- Biggest manual charge: **Chassis (3)** — #1 conversion target.
- Approved by: Andrea:3 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Chassis | per day — $40 (2x), $45 (1x) | days the chassis is held | manual |
| YARD STORAGE | per day — $40 (2x), $45 (1x) | yard dwell days | manual |
| Base Price | flat per move — $865 (1x), $450 (1x) | the lane (load itself) | manual |
| EMPTY LOAD STORAGE | per day — $40 (1x), $45 (1x) | empty dwell days | manual |
| PrePull | flat — $150 (1x), $60 (1x) | pull date vs last-free-day | manual |
| WEEKEND PICK-UP | confirm — $90 (1x) | confirm source | manual |
| DELIVERY DETENTION | per hour after free — $70 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| transload | flat — $575 (1x) | transload job | manual |
| other | unknown — $15 (1x) | unknown | manual |

## Definitions & deviations

- **Chassis** — Chassis usage. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **WEEKEND PICK-UP** — (confirm meaning with carrier). *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
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

- All 3 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Chassis** — 3/3 sets · $40(2x), $45(1x) · manual

> Loads: JYCT_E107813, JYCT_M107089, JYCT_M107937

**YARD STORAGE** — 3/3 sets · $40(2x), $45(1x) · manual

> Loads: JYCT_E107813, JYCT_M107089, JYCT_M107937

**Base Price** — 3/3 sets · $865(1x), $450(1x), $650(1x) · manual

> Loads: JYCT_E107813, JYCT_M107089, JYCT_M107937

**EMPTY LOAD STORAGE** — 2/3 sets · $40(1x), $45(1x) · manual

> Loads: JYCT_E107813, JYCT_M107089

**PrePull** — 2/3 sets · $150(1x), $60(1x) · manual

> Loads: JYCT_M107089, JYCT_M107937

**WEEKEND PICK-UP** — 1/3 sets · $90(1x) · manual

> Loads: JYCT_M107937

**DELIVERY DETENTION** — 1/3 sets · $70(1x) · manual

> Loads: JYCT_M107937

**transload** — 1/3 sets · $575(1x) · manual

> Loads: JYCT_M107937

**other** — 1/3 sets · $15(1x) · manual

> Loads: JYCT_E107813

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_E107813 | EXPORT | PAID | $710 | 5 | Andrea | no |
| JYCT_M107089 | IMPORT | PAID | $1025 | 5 | Andrea | no |
| JYCT_M107937 | IMPORT | PAID | $1250.6 | 7 | Andrea | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
