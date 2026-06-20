---
type: rollup
tags: [rollup, jyc-trucking, charge-auto-approval, scoreboard]
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# JYC Trucking — Carrier-Wide Rollup (contract customers)

Carrier-level deliverable for the [[Charge Auto-Approval Task]], gold-standard "Best Drayage" shape. Aggregates the 260 contract customers (see [[Customers With Contract — JYC Trucking]]); non-contract out of scope ([[All Non-Contract Customers]]).

## Carrier summary

- Contract customers scanned: **260** (0 errors).
- Active (have billed charge sets): **68**; inactive (none on file): **192**.
- Total charge sets: **434**; charge lines: **2593** (191 from tariff, 2402 manual).
- **Carrier-wide auto-approvable today: 7%.** The rest is hand-keyed — the conversion runway.
- Customers sitting at **0% auto: 50** of 68 active — contract exists but no tariff is wired.

**Biggest manual charges carrier-wide (top conversion targets):**
- Chassis — 138 manual lines (as the #1 manual charge across customers)
- Base Price — 119 manual lines (as the #1 manual charge across customers)
- WAREHOUSE STORGE — 46 manual lines (as the #1 manual charge across customers)
- PORT DETENTION — 45 manual lines (as the #1 manual charge across customers)
- PrePull — 24 manual lines (as the #1 manual charge across customers)
- other — 19 manual lines (as the #1 manual charge across customers)
- bonded_cargo_charge — 15 manual lines (as the #1 manual charge across customers)
- pier_pass — 10 manual lines (as the #1 manual charge across customers)

> Reading: JYC has contracts on file for these 260 but the tariffs are largely unwired — so AI auto-approval is near zero today. The fix is mechanical: wire the recurring manual charges (Chassis, Base Price, PrePull, storage, detention, bobtail) into customer tariffs, then define trusted sources for the time/day charges.

## Automatable scoreboard (active customers, by volume)

| Customer                                                | From tariff | Manual | % auto | Biggest manual charge    | Contract                       |
| ------------------------------------------------------- | ----------- | ------ | ------ | ------------------------ | ------------------------------ |
| RT Express USA                                          | 2           | 227    | 1%     | Chassis (48)             | papered · tariff partial (1%)  |
| Arpin International Group - Allyson                     | 19          | 168    | 10%    | Base Price (34)          | papered · tariff partial (10%) |
| BWS LOGISTICS - Freddy                                  | 55          | 109    | 34%    | WAREHOUSE STORGE (46)    | papered · tariff partial (34%) |
| LOGIFLEX CARGO - John                                   | 0           | 114    | 0%     | Base Price (24)          | papered · no tariff            |
| Vibra Finish Company                                    | 0           | 105    | 0%     | Chassis (13)             | papered · no tariff            |
| Ship CXC Inc - Carter                                   | 8           | 90     | 8%     | pier_pass (8)            | papered · tariff partial (8%)  |
| Lookout Global Logistics                                | 16          | 79     | 17%    | Chassis (13)             | papered · tariff partial (17%) |
| CARGO LINK USA LLC - SOPHIE                             | 3           | 80     | 4%     | bonded_cargo_charge (12) | papered · tariff partial (4%)  |
| YING INTERNATIONAL TRANSPORTATION                       | 0           | 83     | 0%     | Chassis (18)             | papered · no tariff            |
| Wheelhouse Logistics                                    | 0           | 80     | 0%     | Base Price (8)           | papered · no tariff            |
| Naz Apparel - Nathan                                    | 0           | 79     | 0%     | PrePull (14)             | papered · no tariff            |
| Island Way Sorbet - Jared                               | 0           | 79     | 0%     | PORT DETENTION (14)      | papered · no tariff            |
| HPL Apollo - Norgi                                      | 8           | 63     | 11%    | Base Price (8)           | papered · tariff partial (11%) |
| US Logistics - Noah                                     | 0           | 67     | 0%     | other (15)               | papered · no tariff            |
| White Oak Logistics - Justin                            | 0           | 64     | 0%     | Chassis (11)             | papered · no tariff            |
| RDS Capacity Solutions                                  | 0           | 64     | 0%     | PORT DETENTION (11)      | papered · no tariff            |
| Quick Pick Logistics LLC-*B                             | 0           | 56     | 0%     | Base Price (9)           | papered · no tariff            |
| Value Chain Logistics (VCL)                             | 0           | 50     | 0%     | PrePull (5)              | papered · no tariff            |
| GOLD COAST LOGISTICS - PRIORITY 1                       | 6           | 42     | 12%    | Base Price (4)           | papered · tariff partial (12%) |
| LEX LOGISTICS                                           | 0           | 44     | 0%     | PORT DETENTION (7)       | papered · no tariff            |
| Freight Services - Marisa                               | 1           | 41     | 2%     | PIER PASS ADMIN FEE (4)  | papered · tariff partial (2%)  |
| Patton Logistical Services LLC                          | 0           | 40     | 0%     | DELIVERY DETENTION (6)   | papered · no tariff            |
| CAPITAL LOGISTICS INC                                   | 0           | 38     | 0%     | PORT DETENTION (8)       | papered · no tariff            |
| AMERICAN CARGO INC-JENNA                                | 14          | 22     | 39%    | Base Price (5)           | papered · tariff partial (39%) |
| JK Solutions Pro - Beryl                                | 8           | 20     | 29%    | Base Price (3)           | papered · tariff partial (29%) |
| LadyMex Grower INC                                      | 4           | 24     | 14%    | HEAVY CONTAINER FEE (3)  | papered · tariff partial (14%) |
| NEM SOLUTIONS LLC - PRIORITY 1                          | 0           | 26     | 0%     | Base Price (4)           | papered · no tariff            |
| Sunny International Logistics Inc                       | 16          | 10     | 62%    | bonded_cargo_charge (3)  | papered · tariff partial (62%) |
| MAC CUSTOMS BROKERAGE                                   | 0           | 23     | 0%     | PrePull (5)              | papered · no tariff            |
| Cranston Trucking - Andee                               | 0           | 21     | 0%     | Chassis (3)              | papered · no tariff            |
| POLO 4PL                                                | 8           | 12     | 40%    | Base Price (2)           | papered · tariff partial (40%) |
| Compass Logistics International                         | 0           | 19     | 0%     | Base Price (4)           | papered · no tariff            |
| Allmodes Logistics Consultancy                          | 0           | 19     | 0%     | PORT DETENTION (3)       | papered · no tariff            |
| ANCHOR LOGISTICS SOLUTIONS                              | 0           | 19     | 0%     | Chassis (2)              | papered · no tariff            |
| Modaltrade USA, Inc - George                            | 0           | 19     | 0%     | Chassis (4)              | papered · no tariff            |
| CALOP FREIGHT SERVICES - CRISTIAN                       | 0           | 19     | 0%     | Chassis (2)              | papered · no tariff            |
| Apostrophe Home                                         | 0           | 19     | 0%     | Chassis (2)              | papered · no tariff            |
| LinksMaster Logistics Co                                | 7           | 11     | 39%    | Base Price (3)           | papered · tariff partial (39%) |
| Worldwide Ocean & Air Shipping Lines Inc                | 0           | 17     | 0%     | Chassis (3)              | papered · no tariff            |
| Twin Teak - Brad                                        | 0           | 17     | 0%     | pier_pass (2)            | papered · no tariff            |
| COSMO LOGISTICS LLC                                     | 0           | 16     | 0%     | Base Price (3)           | papered · no tariff            |
| Clean Earth Logistics - Paul                            | 0           | 15     | 0%     | Chassis (2)              | papered · no tariff            |
| Southland Brokerage Company, Inc                        | 0           | 13     | 0%     | PORT DETENTION (2)       | papered · no tariff            |
| VIATRANZ                                                | 0           | 12     | 0%     | Chassis (4)              | papered · no tariff            |
| ADVIK INC - Clint                                       | 0           | 12     | 0%     | Base Price (1)           | papered · no tariff            |
| Lion Heart Expedited                                    | 0           | 12     | 0%     | DELIVERY DETENTION (3)   | papered · no tariff            |
| STI Products LLC                                        | 0           | 11     | 0%     | other (4)                | papered · no tariff            |
| ASAP Logistics, LLC                                     | 0           | 11     | 0%     | Base Price (1)           | papered · no tariff            |
| Billy - Nationwide Transportation Distribution Services | 0           | 10     | 0%     | Chassis (2)              | papered · no tariff            |
| PINK TRANSPORT USA                                      | 0           | 9      | 0%     | Base Price (2)           | papered · no tariff            |
| Circle Logistics INC                                    | 0           | 9      | 0%     | Chassis (1)              | papered · no tariff            |
| GK Truckline Brokerage                                  | 5           | 4      | 56%    | Base Price (1)           | papered · tariff partial (56%) |
| 1ST CHOICE FREIGHT SERVICE INC                          | 8           | 0      | 100%   | —                        | papered · tariff full          |
| FIFTH WHEEL FREIGHT LLC (FWF)                           | 3           | 5      | 38%    | Chassis (1)              | papered · tariff partial (38%) |
| FREIGHT FLOW SOLUTIONS - PAUL                           | 0           | 7      | 0%     | Chassis (1)              | papered · no tariff            |
| Nationwide Transport Service - Justin                   | 0           | 7      | 0%     | Chassis (1)              | papered · no tariff            |
| JJ UNITED CARGO - PETER                                 | 0           | 7      | 0%     | Chassis (1)              | papered · no tariff            |
| YOG GLOBAL ENTERPRISES, LLC                             | 0           | 7      | 0%     | Chassis (1)              | papered · no tariff            |
| SeaMates International Inc.                             | 0           | 7      | 0%     | Chassis (1)              | papered · no tariff            |
| C-AIR CUSTOMHOUSE BROKERS                               | 0           | 6      | 0%     | Chassis (1)              | papered · no tariff            |
| Global Grid Logistics - LIAN                            | 0           | 6      | 0%     | Chassis (1)              | papered · no tariff            |
| UUL Global US                                           | 0           | 6      | 0%     | Chassis (1)              | papered · no tariff            |
| HD Shipping Solutions - Hunter                          | 0           | 6      | 0%     | Base Price (1)           | papered · no tariff            |
| INDIGENOUS BY LA RUE - Brittany                         | 0           | 6      | 0%     | DELIVERY DETENTION (1)   | papered · no tariff            |
| Total Transport Logistics Inc - Gabe                    | 0           | 6      | 0%     | flip charge (1)          | papered · no tariff            |
| Titan Concepts International - Raul                     | 0           | 5      | 0%     | Chassis (1)              | papered · no tariff            |
| Ronco Freight International Inc                         | 0           | 5      | 0%     | Base Price (1)           | papered · no tariff            |
| Hyland Freight - Henry                                  | 0           | 3      | 0%     | Base Price (1)           | papered · no tariff            |

*(Plus 192 contract customers with no charge sets on file — listed in scan data, omitted here.)*

## 1. Missing-contracts / unwired-tariff list (DTP prerequisite)
All 260 have a signed Customer Contract. The gap is **tariff wiring**: customers at low % auto have the paper but the rates aren't in a tariff. Priority = the high-volume, low-%-auto customers at the top of the scoreboard (RT Express, Logiflex, Ying, Naz Apparel, Vibra, White Oak — all near 0% despite volume).

## 2. Deviations to flag
- BWS — two-clock detention (Port vs Delivery, never merge); 3 storage types; Warehouse Storage two regimes; undefined "$6 other".
- Cargo Link — bonded_cargo_charge is its #1 manual line (bonded-freight handling).
- *(fill as each guide is written)*

## 3. Tariff recommendations & duplicate cleanups
- Carrier-wide: **Chassis** is the single most common biggest-manual charge — wire a per-day Chassis tariff line on every customer where it re-keys.
- **Base Price** hand-keyed on several (Arpin, Logiflex, Quick Pick) — wire the lane price so line haul auto-approves.
- Watch the bobtail / stop-off stacking signal as more customers are processed.

## 4. Auto-approve now vs blocked on a trusted source
- **Now:** line haul + fuel + clean flat contracted lines, the moment a tariff carries them + system approval is on.
- **Blocked on source:** detention (trusted clock), storage/per-diem (dwell/last-free-day), bobtail (trigger), stop-off (count).

## Validation policy
Replay each customer's past loads (would AI produce the charges billers got paid on?), or shadow-forward where no clean history.
