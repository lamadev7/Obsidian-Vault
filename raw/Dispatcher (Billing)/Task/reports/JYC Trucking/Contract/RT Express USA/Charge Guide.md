---
type: report
tags: [charge-guide, jyc-trucking, auto-approval, contract-customer]
customer: RT Express USA
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — RT Express USA (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. Contract customer (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Rolls up to [[Carrier-Wide Rollup]].

## Automatable split (the scoreboard)

**1% auto.** Of 229 charge lines, **2 from the tariff** (auto-approvable) and **227 hand-keyed**. *(observed)*

- Charge sets: 50 (PAID:46, DRAFT:2, INVOICED:2).
- Load types: IMPORT:50.
- Biggest manual charge: **Base Price (48)** — #1 conversion target.
- Approved by: Luciano:50 (all manual — 0 auto-approved).

## Charges — Source / Calculation / Approval

| Charge | Calculation (rate) | Source — where the quantity comes from | Tariff / manual |
|---|---|---|---|
| Base Price | flat per move — $500 (48x), $1 (2x) | the lane (load itself) | mixed (2t/48m) |
| Chassis | per day — $50 (48x) | days the chassis is held | manual |
| YARD STORAGE | per day — $50 (43x) | yard dwell days | manual |
| PrePull | flat — $150 (43x) | pull date vs last-free-day | manual |
| PORT DETENTION | per hour after free — $70 (25x) | terminal gate-in/out clock (TIR/EIR), cross-checked vs driver | manual |
| DELIVERY DETENTION | per hour after free — $70 (11x) | consignee arrive/depart clock, cross-checked vs driver | manual |
| EMPTY LOAD STORAGE | per day — $50 (6x) | empty dwell days | manual |
| flip charge | flat — $55 (3x) | flip move evidence | manual |

## Definitions & deviations

- **Base Price** — Line haul for the move. *(inferred)*
- **Chassis** — Chassis usage. *(inferred)*
- **YARD STORAGE** — Container dwelling at the carrier yard. *(inferred)*
- **PrePull** — Container pulled before last-free-day. *(inferred)*
- **PORT DETENTION** — Time held at the terminal/port. *(inferred)*
- **DELIVERY DETENTION** — Time held at the consignee. *(inferred)*
- **EMPTY LOAD STORAGE** — Empty awaiting return. *(inferred)*
- **flip charge** — Flip charge. *(inferred)*

## Positive charge — the auto-approvable baseline

- Charges come only from the menu above at the known rates; an unknown name or off-rate line → hold and review.
- Time/day lines (detention, storage, per-diem) carry a quantity from a trusted source before auto-approval; until then manual-review.
- Any UNDEFINED line (e.g. 'other'/'misc') → always hold and escalate.

## Contract & tariff status

- Has a signed Customer Contract on file. *(observed)*
- **Tariff barely wired** (1%) — most lines hand-keyed. *(observed)*

## Validate against history

- All 50 charge sets reviewed; rates as listed. Every set human-approved — **0% auto-approved today.** *(observed)*

## Evidence (from billing history)

### A. Each charge → the loads it appears on

**Base Price** — 50/50 sets · $500(48x), $1(2x) · 2t/48m

> Loads: JYCT_M107359, JYCT_M107461, JYCT_M107468, JYCT_M107469, JYCT_M107470, JYCT_M107471, JYCT_M107512, JYCT_M107513, JYCT_M107514, JYCT_M107515, JYCT_M107516, JYCT_M107601, JYCT_M107602, JYCT_M107609, JYCT_M107610, JYCT_M107618, JYCT_M107619, JYCT_M107620, JYCT_M107653, JYCT_M107674, JYCT_M107675, JYCT_M107676, JYCT_M107677, JYCT_M107678, JYCT_M107679, JYCT_M107680, JYCT_M107752, JYCT_M107753, JYCT_M107754, JYCT_M107784, JYCT_M107785, JYCT_M107786, JYCT_M107815, JYCT_M107816, JYCT_M107864, JYCT_M107930, JYCT_M107931, JYCT_M107932, JYCT_M107933, JYCT_M107934, JYCT_M108002, JYCT_M108306, JYCT_M108350, JYCT_M108370, JYCT_M108506, JYCT_M108668, JYCT_M108697, JYCT_M108698, JYCT_M108797, JYCT_M108819

**Chassis** — 48/50 sets · $50(48x) · manual

> Loads: JYCT_M107359, JYCT_M107461, JYCT_M107468, JYCT_M107469, JYCT_M107470, JYCT_M107471, JYCT_M107512, JYCT_M107513, JYCT_M107514, JYCT_M107515, JYCT_M107516, JYCT_M107601, JYCT_M107602, JYCT_M107609, JYCT_M107610, JYCT_M107618, JYCT_M107619, JYCT_M107620, JYCT_M107653, JYCT_M107674, JYCT_M107675, JYCT_M107676, JYCT_M107677, JYCT_M107678, JYCT_M107679, JYCT_M107680, JYCT_M107752, JYCT_M107753, JYCT_M107754, JYCT_M107784, JYCT_M107785, JYCT_M107786, JYCT_M107815, JYCT_M107816, JYCT_M107864, JYCT_M107930, JYCT_M107931, JYCT_M107932, JYCT_M107933, JYCT_M107934, JYCT_M108002, JYCT_M108306, JYCT_M108350, JYCT_M108370, JYCT_M108506, JYCT_M108668, JYCT_M108697, JYCT_M108698

**YARD STORAGE** — 43/50 sets · $50(43x) · manual

> Loads: JYCT_M107359, JYCT_M107461, JYCT_M107468, JYCT_M107469, JYCT_M107470, JYCT_M107471, JYCT_M107512, JYCT_M107513, JYCT_M107514, JYCT_M107515, JYCT_M107516, JYCT_M107601, JYCT_M107602, JYCT_M107609, JYCT_M107610, JYCT_M107618, JYCT_M107619, JYCT_M107674, JYCT_M107675, JYCT_M107676, JYCT_M107677, JYCT_M107678, JYCT_M107679, JYCT_M107680, JYCT_M107752, JYCT_M107753, JYCT_M107754, JYCT_M107784, JYCT_M107785, JYCT_M107786, JYCT_M107815, JYCT_M107816, JYCT_M107930, JYCT_M107931, JYCT_M107932, JYCT_M107933, JYCT_M107934, JYCT_M108002, JYCT_M108306, JYCT_M108350, JYCT_M108506, JYCT_M108668, JYCT_M108698

**PrePull** — 43/50 sets · $150(43x) · manual

> Loads: JYCT_M107359, JYCT_M107461, JYCT_M107468, JYCT_M107469, JYCT_M107470, JYCT_M107471, JYCT_M107512, JYCT_M107513, JYCT_M107514, JYCT_M107515, JYCT_M107516, JYCT_M107601, JYCT_M107602, JYCT_M107609, JYCT_M107610, JYCT_M107618, JYCT_M107619, JYCT_M107674, JYCT_M107675, JYCT_M107676, JYCT_M107677, JYCT_M107678, JYCT_M107679, JYCT_M107680, JYCT_M107752, JYCT_M107753, JYCT_M107754, JYCT_M107784, JYCT_M107785, JYCT_M107786, JYCT_M107815, JYCT_M107816, JYCT_M107930, JYCT_M107931, JYCT_M107932, JYCT_M107933, JYCT_M107934, JYCT_M108002, JYCT_M108306, JYCT_M108350, JYCT_M108506, JYCT_M108668, JYCT_M108698

**PORT DETENTION** — 21/50 sets · $70(25x) · manual

> Loads: JYCT_M107461, JYCT_M107470, JYCT_M107512, JYCT_M107513, JYCT_M107601, JYCT_M107602, JYCT_M107610, JYCT_M107619, JYCT_M107620, JYCT_M107676, JYCT_M107678, JYCT_M107753, JYCT_M107786, JYCT_M107816, JYCT_M107931, JYCT_M107932, JYCT_M108370, JYCT_M108506, JYCT_M108668, JYCT_M108697, JYCT_M108698

**DELIVERY DETENTION** — 11/50 sets · $70(11x) · manual

> Loads: JYCT_M107468, JYCT_M107515, JYCT_M107516, JYCT_M107609, JYCT_M107619, JYCT_M107620, JYCT_M107653, JYCT_M107679, JYCT_M107786, JYCT_M107931, JYCT_M108306

**EMPTY LOAD STORAGE** — 6/50 sets · $50(6x) · manual

> Loads: JYCT_M107469, JYCT_M107470, JYCT_M107515, JYCT_M107930, JYCT_M108370, JYCT_M108698

**flip charge** — 3/50 sets · $55(3x) · manual

> Loads: JYCT_M107512, JYCT_M107516, JYCT_M107610

### B. Chargeset-level evidence

| Load ref | Type | Status | Total | # charges | Approved by | Tariff-rated |
|---|---|---|---|---|---|---|
| JYCT_M107359 | IMPORT | PAID | $850 | 4 | Luciano | no |
| JYCT_M107461 | IMPORT | PAID | $1034.5 | 5 | Luciano | no |
| JYCT_M107468 | IMPORT | PAID | $867.5 | 5 | Luciano | no |
| JYCT_M107469 | IMPORT | PAID | $950 | 5 | Luciano | no |
| JYCT_M107470 | IMPORT | PAID | $1000.4 | 6 | Luciano | no |
| JYCT_M107471 | IMPORT | PAID | $1050 | 4 | Luciano | no |
| JYCT_M107512 | IMPORT | PAID | $1026.7 | 6 | Luciano | no |
| JYCT_M107513 | IMPORT | PAID | $1248.1 | 5 | Luciano | no |
| JYCT_M107514 | IMPORT | PAID | $850 | 4 | Luciano | no |
| JYCT_M107515 | IMPORT | PAID | $1267.5 | 6 | Luciano | no |
| JYCT_M107516 | IMPORT | PAID | $935.1 | 6 | Luciano | no |
| JYCT_M107601 | IMPORT | PAID | $1190 | 5 | Luciano | no |
| JYCT_M107602 | IMPORT | PAID | $1170.1 | 5 | Luciano | no |
| JYCT_M107609 | IMPORT | PAID | $861.2 | 5 | Luciano | no |
| JYCT_M107610 | IMPORT | PAID | $1226 | 6 | Luciano | no |
| JYCT_M107618 | IMPORT | PAID | $950 | 4 | Luciano | no |
| JYCT_M107619 | IMPORT | PAID | $1102.5 | 6 | Luciano | no |
| JYCT_M107620 | IMPORT | PAID | $684.4 | 4 | Luciano | no |
| JYCT_M107653 | IMPORT | PAID | $566.1 | 3 | Luciano | no |
| JYCT_M107674 | IMPORT | PAID | $950 | 4 | Luciano | no |
| JYCT_M107675 | IMPORT | PAID | $850 | 4 | Luciano | no |
| JYCT_M107676 | IMPORT | PAID | $913 | 5 | Luciano | no |
| JYCT_M107677 | IMPORT | PAID | $850 | 4 | Luciano | no |
| JYCT_M107678 | IMPORT | PAID | $930.5 | 5 | Luciano | no |
| JYCT_M107679 | IMPORT | PAID | $870 | 5 | Luciano | no |
| JYCT_M107680 | IMPORT | PAID | $850 | 4 | Luciano | no |
| JYCT_M107752 | IMPORT | PAID | $1050 | 4 | Luciano | no |
| JYCT_M107753 | IMPORT | PAID | $866.1 | 5 | Luciano | no |
| JYCT_M107754 | IMPORT | PAID | $750 | 4 | Luciano | no |
| JYCT_M107784 | IMPORT | PAID | $750 | 4 | Luciano | no |
| JYCT_M107785 | IMPORT | PAID | $850 | 4 | Luciano | no |
| JYCT_M107786 | IMPORT | PAID | $886.8 | 6 | Luciano | no |
| JYCT_M107815 | IMPORT | PAID | $900 | 4 | Luciano | no |
| JYCT_M107816 | IMPORT | PAID | $823.1 | 5 | Luciano | no |
| JYCT_M107864 | IMPORT | PAID | $550 | 2 | Luciano | no |
| JYCT_M107930 | IMPORT | PAID | $950 | 5 | Luciano | no |
| JYCT_M107931 | IMPORT | PAID | $883.6 | 6 | Luciano | no |
| JYCT_M107932 | IMPORT | PAID | $945.2 | 5 | Luciano | no |
| JYCT_M107933 | IMPORT | PAID | $850 | 4 | Luciano | no |
| JYCT_M107934 | IMPORT | PAID | $850 | 4 | Luciano | no |
| JYCT_M108002 | IMPORT | PAID | $850 | 4 | Luciano | no |
| JYCT_M108306 | IMPORT | PAID | $852.5 | 5 | Luciano | no |
| JYCT_M108350 | IMPORT | PAID | $750 | 4 | Luciano | no |
| JYCT_M108370 | IMPORT | PAID | $622.4 | 4 | Luciano | no |
| JYCT_M108506 | IMPORT | PAID | $859.5 | 6 | Luciano | no |
| JYCT_M108668 | IMPORT | INVOICED | $850 | 6 | Luciano | no |
| JYCT_M108697 | IMPORT | PAID | $550 | 4 | Luciano | no |
| JYCT_M108698 | IMPORT | INVOICED | $850 | 7 | Luciano | no |
| JYCT_M108797 | IMPORT | DRAFT | $1 | 1 | Luciano | yes |
| JYCT_M108819 | IMPORT | DRAFT | $1 | 1 | Luciano | yes |

---
*Derivation tags: (observed) from billing history · (inferred) one step beyond · (assumed) standard practice, confirm.*
