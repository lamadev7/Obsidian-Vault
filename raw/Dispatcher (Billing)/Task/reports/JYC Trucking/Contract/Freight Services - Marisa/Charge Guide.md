---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: Freight Services - Marisa
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — Freight Services - Marisa (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**2% auto.** Of 42 charge lines, **1 from the tariff** (auto-approvable) and **41 hand-keyed**. *(observed)*

- Charge sets: 4 (PAID:4).
- Load types: IMPORT:4.
- Biggest manual charge: **PIER PASS ADMIN FEE (4)** — #1 conversion target.
- Approved by: Miguel:4 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| PIER PASS ADMIN FEE | flat pass-through — $50 (4x) | pass-through (port receipt) | manual |
| bonded_cargo_charge | flat — $150 (4x) | bonded move flag | manual |
| Hazmat | confirm — $150 (4x) | confirm source | manual |
| Base Price | flat per move — $967.5 (4x) | the lane (load itself) | manual |
| PrePull | flat — $150 (4x) | pull date vs last-free-day | manual |
| YARD STORAGE | per day — $45 (4x) | yard dwell days | manual |
| Chassis | per day — $45 (4x) | days the chassis is held | manual |
| pier_pass | flat pass-through — $39 (2x), $78 (2x) | pass-through (port receipt) | manual |
| CLEAN TRUCK FEE | confirm — $10 (2x), $20 (2x) | confirm source | manual |
| scale_load | flat — $14 (2x) | scale ticket | manual |
| EMPTY LOAD STORAGE | per day — $60 (1x), $45 (1x) | empty dwell days | mixed (1t/1m) |
| PORT DETENTION | per hour after free — $70 (2x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |

## Definitions & deviations

- **PIER PASS ADMIN FEE** — Pier Pass / port fee. *(inferred)*
- **bonded_cargo_charge** — Bonded-cargo handling. *(inferred)*
- **Hazmat** — (confirm meaning with carrier). *(inferred)*
- **Base Price** — Line haul for the move. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **pier_pass** — Pier Pass / port fee. *(inferred)*
- **CLEAN TRUCK FEE** — (confirm meaning with carrier). *(inferred)*
- **scale_load** — Scale / weigh. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (2%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 4 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**PIER PASS ADMIN FEE** — 4/4 sets · $50(4x) · manual

> Loads: JYCT_M107337, JYCT_M107338, JYCT_M107414, JYCT_M107428

**bonded_cargo_charge** — 4/4 sets · $150(4x) · manual

> Loads: JYCT_M107337, JYCT_M107338, JYCT_M107414, JYCT_M107428

**Hazmat** — 4/4 sets · $150(4x) · manual

> Loads: JYCT_M107337, JYCT_M107338, JYCT_M107414, JYCT_M107428

**Base Price** — 4/4 sets · $967.5(4x) · manual

> Loads: JYCT_M107337, JYCT_M107338, JYCT_M107414, JYCT_M107428

**PrePull** — 4/4 sets · $150(4x) · manual

> Loads: JYCT_M107337, JYCT_M107338, JYCT_M107414, JYCT_M107428

**YARD STORAGE** — 4/4 sets · $45(4x) · manual

> Loads: JYCT_M107337, JYCT_M107338, JYCT_M107414, JYCT_M107428

**Chassis** — 4/4 sets · $45(4x) · manual

> Loads: JYCT_M107337, JYCT_M107338, JYCT_M107414, JYCT_M107428

**pier_pass** — 4/4 sets · $39(2x), $78(2x) · manual

> Loads: JYCT_M107337, JYCT_M107338, JYCT_M107414, JYCT_M107428

**CLEAN TRUCK FEE** — 4/4 sets · $10(2x), $20(2x) · manual

> Loads: JYCT_M107337, JYCT_M107338, JYCT_M107414, JYCT_M107428

**scale_load** — 2/4 sets · $14(2x) · manual

> Loads: JYCT_M107338, JYCT_M107428

**EMPTY LOAD STORAGE** — 2/4 sets · $60(1x), $45(1x) · 1t/1m

> Loads: JYCT_M107338, JYCT_M107428

**PORT DETENTION** — 2/4 sets · $70(2x) · manual

> Loads: JYCT_M107337, JYCT_M107338

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107337 | IMPORT | PAID | $1914 | 10 | Miguel | no |
| JYCT_M107338 | IMPORT | PAID | $1938.2 | 12 | Miguel | no |
| JYCT_M107414 | IMPORT | PAID | $1876.5 | 9 | Miguel | no |
| JYCT_M107428 | IMPORT | PAID | $1770.5 | 11 | Miguel | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
