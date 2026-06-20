---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: STI Products LLC
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — STI Products LLC (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**0% auto.** Of 11 charge lines, **0 from the tariff** (auto-approvable) and **11 hand-keyed**. *(observed)*

- Charge sets: 3 (PAID:3).
- Load types: BILL_ONLY:2, IMPORT:1.
- Biggest manual charge: **other (4)** — #1 conversion target.
- Approved by: Juan Andres:2, Juan:1 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| other | unknown — $525 (1x), $20 (1x) | unknown | manual |
| DELIVERY DETENTION | per hour after free — $50 (1x), $75 (1x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| Detention | per hour after free — $50 (1x) | arrive/depart clock (name the location), cross-checked vs driver | manual |
| Base Price | flat per move — $1 (1x) | the lane (load itself) | manual |
| YARD STORAGE | per day — $45 (1x) | yard dwell days | manual |
| tri_axle | confirm — $95 (1x) | confirm source | manual |
| transload | flat — $525 (1x) | transload job | manual |

## Definitions & deviations

- **other** — UNDEFINED line — must be identified before trust. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **Detention** — Detention wait time. *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **tri_axle** — (confirm meaning with carrier). *(inferred)*
- **transload** — Cross-dock/transload service. *(inferred)*

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

**other** — 3/3 sets · $525(1x), $20(1x), $575(1x) · manual

> Loads: JYCT_B108579, JYCT_B108606, JYCT_M108456

**DELIVERY DETENTION** — 2/3 sets · $50(1x), $75(1x) · manual

> Loads: JYCT_B108579, JYCT_M108456

**Detention** — 1/3 sets · $50(1x) · manual

> Loads: JYCT_B108606

**Base Price** — 1/3 sets · $1(1x) · manual

> Loads: JYCT_M108456

**YARD STORAGE** — 1/3 sets · $45(1x) · manual

> Loads: JYCT_M108456

**tri_axle** — 1/3 sets · $95(1x) · manual

> Loads: JYCT_M108456

**transload** — 1/3 sets · $525(1x) · manual

> Loads: JYCT_M108456

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_B108579 | BILL_ONLY | PAID | $933.5 | 2 | Juan | no |
| JYCT_B108606 | BILL_ONLY | PAID | $766 | 3 | Juan Andres | no |
| JYCT_M108456 | IMPORT | PAID | $1496.25 | 6 | Juan Andres | no |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
