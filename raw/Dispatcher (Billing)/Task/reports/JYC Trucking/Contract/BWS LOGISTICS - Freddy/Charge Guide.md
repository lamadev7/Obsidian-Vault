---
type: report
tags: [charge-guide, jyc-trucking, bws-logistics, auto-approval, contract-customer]
customer: BWS LOGISTICS - Freddy
carrier: JYC Trucking (carlos@jyctrucking.com)
updated: 2026-06-01
---

# Charge Guide — BWS LOGISTICS - Freddy (JYC Trucking)

Charge guide for the [[Charge Auto-Approval Task]]. Built from this customer's real billing history. A **contract customer** (has a Customer Contract on file — see [[Customers With Contract — JYC Trucking]]). Concepts in [[Dispatcher Billing — Overview]].

> **Scope:** all 50 chargesets on file (39 invoiced, 11 still in draft). Contact: Freddy (f.leon@jyctracking.com). Consolidated into one document; can be split into the `Findings/` file layout on request.

---

## Automatable split (the scoreboard)

**34% auto.** Of 164 charge lines on file, **55 come from the tariff** (auto-approvable) and **109 are hand-keyed** (not automatable until their source + calculation are wired into a tariff line). *(observed)*

- **Biggest manual charge:** Warehouse Storage (46 lines) — the #1 conversion target.
- **Contract status:** has a signed Customer Contract; a tariff exists but only the **import-drayage** side is wired. The **bill-only** side is papered-but-not-tariffed.
- **The number to move:** every manual line converted to a tariff line raises this %. The 109 manual lines concentrate in Warehouse Storage, Transload, and the two Detentions — convert those and BWS jumps toward the 90% the easy charges already allow.

---

## 0. Customer at a glance

- **Two business lines, not one:** about a third of the work is **import drayage**; the rest is **bill-only** (warehouse storage / transload — no truck move, just billing). Treat them as two different charge profiles on the same customer. *(observed)*
- **A tariff exists and is partly wired** — the "BWS Logistics" tariff actively rates the import-drayage charges; the warehouse/transload side is entered by hand. *(observed)*
- **Nothing auto-approves today** — every chargeset is approved by a person (Fernando on most, Miguel on the rest); the system auto-approved zero. *(observed)*

---

## 1. Line haul

- **Import side:** a single all-in **Base Price** of $540 per move, **rated straight from the BWS Logistics tariff** (not hand-typed) on most loads. *(observed)*
- **All-in — no separate fuel surcharge** on any load. *(observed)*
- **Bill-only side has no line haul** — those chargesets are storage/transload only (no transport leg). *(observed)*
- **Automatable?** The import Base Price already comes from the tariff — it is the closest thing this customer has to an auto-approvable charge today. The only blocker is the approval gate isn't firing automatically. *(inferred)*

---

## 2. Contracts — partly wired, with gaps

Unlike a tariff-less customer, BWS has a working tariff for the import charges — Base Price, Pre-Pull, Chassis, Yard Storage and Empty Load Storage come through **rated from the contract** on the import loads. *(observed)*

**The gaps that keep auto-approval at zero:**
- **Warehouse Storage, Transload, Port/Delivery Detention, Heavy Container Fee, and the "other" line are all hand-entered** — not on the tariff. *(observed)*
- Even the tariff-rated charges still get a **manual approval** — the rate is automatic but the sign-off is not. *(observed)*

**Hand to the implementation / DTP team — add to the tariff:** Warehouse Storage (with its two regimes — see §3), Transload, Port Detention, Delivery Detention, Heavy Container Fee. The drayage side is already encoded; this closes the bill-only side. *(inferred)*

---

## 3. Charges — Source / Calculation / Approval

Every charge this customer is billed. **Calculation** observed from history. **Source** is the open question for the time/quantity-based lines. **Approval** is 100% manual today.

| Charge                            | Calculation *(observed)*                                                                 | Source — where the quantity comes from                                        | Rated from tariff? |
| --------------------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------ |
| **Base Price** (import line haul) | $540 flat per move                                                                       | the lane (import drayage)                                                     | ✅ yes              |
| **Warehouse Storage**             | flat per occurrence — **two regimes**: $52.50 with 7 free days, or $30 with no free days | days in the warehouse; *which regime applies must be pinned down*             | ✗ manual           |
| **Transload**                     | flat $1,050                                                                              | triggered on a transload job (cross-dock between containers)                  | ✗ manual           |
| **Pre-Pull**                      | flat $125                                                                                | pulled ahead of the last-free-day                                             | ✅ mostly           |
| **Chassis**                       | $40 (flat or per-day)                                                                    | days the chassis is held                                                      | ✅ mostly           |
| **Yard Storage**                  | $40/day                                                                                  | days dwelling at the carrier yard                                             | ✅ mostly           |
| **Empty Load Storage**            | $40/day                                                                                  | days an empty waits before return                                             | ✅ mostly           |
| **Port Detention**                | $70/hour, 2 free                                                                         | arrive/depart at the terminal; *confirm geofence vs hand-keyed*               | ✗ manual           |
| **Delivery Detention**            | $70/hour, 2 free                                                                         | arrive/depart at the consignee; *same source question*                        | ✗ manual           |
| **Heavy Container Fee**           | flat $100                                                                                | triggered on an overweight container                                          | ✗ manual           |
| **"other"**                       | flat $6                                                                                  | unclear — small recurring line; **needs definition before it can be trusted** | ✗ manual           |

**Reading of the source problem:** the storage and detention lines are where the number depends on *how long* — and a quantity a biller types can be inflated, so each needs a **trusted, monitored source**, not a hand-keyed count. Name the source per charge: detention hours from **terminal gate-in/gate-out (or TIR/EIR)** cross-checked against the driver clock; storage days from the **dwell/last-free-day record**, not a typed day-count. The flat lines (Base Price, Transload, Pre-Pull, Heavy Container Fee) have no quantity to dispute and can auto-approve once the tariff carries them. The **"other" $6 line must be identified** — an unexplained recurring charge is an auto-approval risk. *(inferred)*

---

## 4. Definitions & deviations

- **Three distinct storage charges** for this customer — **Warehouse Storage** (in the warehouse, bill-only side), **Yard Storage** (container dwelling at the carrier yard), and **Empty Load Storage** (an empty awaiting return). Don't collapse them; they bill separately. *(observed)*
- **Warehouse Storage has two rate regimes** ($52.50 with a 7-day free allowance vs $30 with none) — almost certainly two different storage agreements or container classes. Confirm which trigger selects which regime. *(inferred)*
- **Transload** — a cross-dock/transload service line ($1,050), not a drayage move. Appears on the bill-only chargesets. *(observed)*
- **Port Detention vs Delivery Detention — the two-clock rule (do not merge).** Both bill at the same $70/hr, 2 free — but they are **two different clocks with two different sources**: Port Detention is time **held at the terminal/port**, Delivery Detention is time **held at the consignee**. Same number, different waits — never collapse them into one line. Each needs its own trusted source: terminal **gate-in/gate-out** (or TIR/EIR interchange) for Port Detention, and **arrive/depart at the consignee** for Delivery Detention — cross-checked against the driver-reported clock, never the driver clock alone. *(observed; source assumed)*
- **Heavy Container Fee** — overweight surcharge. *(inferred)*
- **"other"** — undefined $6 line; flag for clarification. *(observed)*

---

## 5. Positive charge — the auto-approvable baseline

A valid BWS chargeset that should auto-approve once contracts + sources are wired:

- **Import load:** carries a Base Price ($540) plus accessorials from the menu at known rates. *(observed)*
- **Bill-only job:** carries Warehouse Storage and/or Transload, no Base Price. *(observed)*
- Charges come **only from the menu above** at the known rates; an unknown charge name or off-rate line → hold and review. *(inferred)*
- **No fuel surcharge** line (fuel is in Base Price). *(observed)*
- Time/day lines (Warehouse/Yard/Empty Storage, Port/Delivery Detention) carry a quantity from a **trusted source** once defined; until then they stay manual-review. *(assumed)*
- The **"other" line is resolved/defined**, or the chargeset holds for review. *(inferred)*

---

## 6. Validate against history

- **All 50 chargesets match the menu** above — no surprise charge names. *(observed)*
- **Rates are consistent:** Base Price $540; Port/Delivery Detention $70/hr 2-free; Chassis/Yard/Empty Storage $40; Pre-Pull $125; Transload $1,050; Heavy Container Fee $100. Warehouse Storage runs on its two regimes. *(observed)*
- **The import-side charges are already tariff-rated** (came from the contract, not hand-typed) — proof the contract works where it's wired. *(observed)*
- **Every chargeset was human-approved and none auto-approved** — auto-approval coverage is 0% here today. *(observed)*
- **11 of 50 are still in draft** — a backlog sitting unbilled; worth flagging operationally. *(observed)*

---

## 7. The opportunity

BWS is further along than a tariff-less customer — the **drayage side is already contract-rated**. Two moves get it to high auto-approval:

1. **Extend the tariff to the bill-only side** — encode Warehouse Storage (both regimes), Transload, the two Detentions, and Heavy Container Fee at the rates above. *(inferred)*
2. **Define the trusted source** for the storage and detention quantities (warehouse days, yard days, terminal/consignee arrive-depart), and **identify the "other" $6 line.** *(inferred)*

Then the flat charges and the already-rated drayage lines auto-approve immediately, and the time-based lines follow once their source is wired.

---

## 8. Evidence (from billing history)

Real data backing every rule, so each charge can be traced to the loads it appears on. Across the 50 chargesets on file. *(observed)*

### A. Each charge → the loads it appears on

**WAREHOUSE STORGE** — on 23/50 chargesets · rate $52.5 (23x), $30 (23x) · manual:46

> Loads: JYCT_B108693, JYCT_B108699, JYCT_B108716, JYCT_B108717, JYCT_B108718, JYCT_B108720, JYCT_B108726, JYCT_B108727, JYCT_B108728, JYCT_B108733, JYCT_B108747, JYCT_B108748, JYCT_B108803, JYCT_B108814, JYCT_B108815, JYCT_B108820, JYCT_B108836, JYCT_B108839, JYCT_B108841, JYCT_B108842, JYCT_B108854, JYCT_B108855, JYCT_B108863

**other** — on 19/50 chargesets · rate $6 (19x) · manual:19

> Loads: JYCT_B108699, JYCT_B108716, JYCT_B108717, JYCT_B108718, JYCT_B108720, JYCT_B108726, JYCT_B108727, JYCT_B108728, JYCT_B108733, JYCT_B108803, JYCT_B108814, JYCT_B108815, JYCT_B108820, JYCT_B108836, JYCT_B108839, JYCT_B108841, JYCT_B108842, JYCT_B108855, JYCT_B108863

**PrePull** — on 15/50 chargesets · rate $125 (15x) · tariff:11, manual:4

> Loads: JYCT_M108740, JYCT_M108741, JYCT_M108742, JYCT_M108743, JYCT_M108823, JYCT_M108824, JYCT_M108825, JYCT_M108826, JYCT_M108827, JYCT_M108828, JYCT_M108829, JYCT_M108830, JYCT_M108831, JYCT_M108832, JYCT_M108833

**Chassis** — on 15/50 chargesets · rate $40 (15x) · tariff:11, manual:4

> Loads: JYCT_M108740, JYCT_M108741, JYCT_M108742, JYCT_M108743, JYCT_M108823, JYCT_M108824, JYCT_M108825, JYCT_M108826, JYCT_M108827, JYCT_M108828, JYCT_M108829, JYCT_M108830, JYCT_M108831, JYCT_M108832, JYCT_M108833

**YARD STORAGE** — on 15/50 chargesets · rate $40 (15x) · tariff:11, manual:4

> Loads: JYCT_M108740, JYCT_M108741, JYCT_M108742, JYCT_M108743, JYCT_M108823, JYCT_M108824, JYCT_M108825, JYCT_M108826, JYCT_M108827, JYCT_M108828, JYCT_M108829, JYCT_M108830, JYCT_M108831, JYCT_M108832, JYCT_M108833

**Base Price** — on 15/50 chargesets · rate $540 (15x) · tariff:11, manual:4

> Loads: JYCT_M108740, JYCT_M108741, JYCT_M108742, JYCT_M108743, JYCT_M108823, JYCT_M108824, JYCT_M108825, JYCT_M108826, JYCT_M108827, JYCT_M108828, JYCT_M108829, JYCT_M108830, JYCT_M108831, JYCT_M108832, JYCT_M108833

**EMPTY LOAD STORAGE** — on 13/50 chargesets · rate $40 (13x) · tariff:11, manual:2

> Loads: JYCT_M108740, JYCT_M108743, JYCT_M108823, JYCT_M108824, JYCT_M108825, JYCT_M108826, JYCT_M108827, JYCT_M108828, JYCT_M108829, JYCT_M108830, JYCT_M108831, JYCT_M108832, JYCT_M108833

**transload** — on 12/50 chargesets · rate $1050 (12x) · manual:12

> Loads: JYCT_B108750, JYCT_B108751, JYCT_B108789, JYCT_B108790, JYCT_B108799, JYCT_B108800, JYCT_B108801, JYCT_B108802, JYCT_B108850, JYCT_B108851, JYCT_B108852, JYCT_B108853

**HEAVY CONTAINER FEE** — on 4/50 chargesets · rate $100 (4x) · manual:4

> Loads: JYCT_M108740, JYCT_M108741, JYCT_M108742, JYCT_M108743

**PORT DETENTION** — on 4/50 chargesets · rate $70 (8x) · manual:8

> Loads: JYCT_M108740, JYCT_M108741, JYCT_M108742, JYCT_M108743

**DELIVERY DETENTION** — on 2/50 chargesets · rate $70 (2x) · manual:2

> Loads: JYCT_M108740, JYCT_M108743

### B. Chargeset-level evidence

| Load ref     | Type      | Status   | Total   | # charges | Approved by | Tariff-rated |
| ------------ | --------- | -------- | ------- | --------- | ----------- | ------------ |
| JYCT_B108693 | BILL_ONLY | INVOICED | $2152.5 | 2         | Fernando    | no           |
| JYCT_B108699 | BILL_ONLY | INVOICED | $2263.5 | 3         | Fernando    | no           |
| JYCT_B108716 | BILL_ONLY | INVOICED | $1948.5 | 3         | Fernando    | no           |
| JYCT_B108717 | BILL_ONLY | INVOICED | $2001   | 3         | Fernando    | no           |
| JYCT_B108718 | BILL_ONLY | INVOICED | $1948.5 | 3         | Fernando    | no           |
| JYCT_B108720 | BILL_ONLY | INVOICED | $1423.5 | 3         | Fernando    | no           |
| JYCT_B108726 | BILL_ONLY | INVOICED | $1843.5 | 3         | Fernando    | no           |
| JYCT_B108727 | BILL_ONLY | INVOICED | $1266   | 3         | Fernando    | no           |
| JYCT_B108728 | BILL_ONLY | INVOICED | $1318.5 | 3         | Fernando    | no           |
| JYCT_B108733 | BILL_ONLY | INVOICED | $1318.5 | 3         | Fernando    | no           |
| JYCT_B108747 | BILL_ONLY | INVOICED | $1680   | 2         | Fernando    | no           |
| JYCT_B108748 | BILL_ONLY | INVOICED | $1837.5 | 2         | Fernando    | no           |
| JYCT_B108750 | BILL_ONLY | INVOICED | $1050   | 1         | Fernando    | no           |
| JYCT_B108751 | BILL_ONLY | INVOICED | $1050   | 1         | Fernando    | no           |
| JYCT_B108789 | BILL_ONLY | INVOICED | $1050   | 1         | Fernando    | no           |
| JYCT_B108790 | BILL_ONLY | INVOICED | $1050   | 1         | Fernando    | no           |
| JYCT_B108799 | BILL_ONLY | INVOICED | $1050   | 1         | Fernando    | no           |
| JYCT_B108800 | BILL_ONLY | INVOICED | $1050   | 1         | Fernando    | no           |
| JYCT_B108801 | BILL_ONLY | INVOICED | $1050   | 1         | Fernando    | no           |
| JYCT_B108802 | BILL_ONLY | INVOICED | $1050   | 1         | Fernando    | no           |
| JYCT_B108803 | BILL_ONLY | INVOICED | $1948.5 | 3         | Fernando    | no           |
| JYCT_B108814 | BILL_ONLY | INVOICED | $2001   | 3         | Fernando    | no           |
| JYCT_B108815 | BILL_ONLY | INVOICED | $2053.5 | 3         | Fernando    | no           |
| JYCT_B108820 | BILL_ONLY | INVOICED | $2001   | 3         | Fernando    | no           |
| JYCT_B108836 | BILL_ONLY | INVOICED | $2001   | 3         | Fernando    | no           |
| JYCT_B108839 | BILL_ONLY | INVOICED | $2053.5 | 3         | Fernando    | no           |
| JYCT_B108841 | BILL_ONLY | INVOICED | $2001   | 3         | Fernando    | no           |
| JYCT_B108842 | BILL_ONLY | INVOICED | $2053.5 | 3         | Fernando    | no           |
| JYCT_B108850 | BILL_ONLY | INVOICED | $1050   | 1         | Fernando    | no           |
| JYCT_B108851 | BILL_ONLY | INVOICED | $1050   | 1         | Fernando    | no           |
| JYCT_B108852 | BILL_ONLY | INVOICED | $1050   | 1         | Fernando    | no           |
| JYCT_B108853 | BILL_ONLY | INVOICED | $1050   | 1         | Fernando    | no           |
| JYCT_B108854 | BILL_ONLY | INVOICED | $1522.5 | 2         | Fernando    | no           |
| JYCT_B108855 | BILL_ONLY | INVOICED | $1791   | 3         | Fernando    | no           |
| JYCT_B108863 | BILL_ONLY | INVOICED | $2001   | 3         | Fernando    | no           |
| JYCT_M108740 | IMPORT    | INVOICED | $914.4  | 9         | Miguel      | no           |
| JYCT_M108741 | IMPORT    | INVOICED | $885    | 7         | Miguel      | no           |
| JYCT_M108742 | IMPORT    | INVOICED | $1125   | 7         | Miguel      | no           |
| JYCT_M108743 | IMPORT    | INVOICED | $1033.5 | 9         | Miguel      | no           |
| JYCT_M108823 | IMPORT    | DRAFT    | $785    | 5         | Miguel      | yes          |
| JYCT_M108824 | IMPORT    | DRAFT    | $785    | 5         | Miguel      | yes          |
| JYCT_M108825 | IMPORT    | DRAFT    | $785    | 5         | Miguel      | yes          |
| JYCT_M108826 | IMPORT    | DRAFT    | $785    | 5         | Miguel      | yes          |
| JYCT_M108827 | IMPORT    | DRAFT    | $785    | 5         | Miguel      | yes          |
| JYCT_M108828 | IMPORT    | DRAFT    | $785    | 5         | Miguel      | yes          |
| JYCT_M108829 | IMPORT    | DRAFT    | $785    | 5         | Miguel      | yes          |
| JYCT_M108830 | IMPORT    | DRAFT    | $785    | 5         | Miguel      | yes          |
| JYCT_M108831 | IMPORT    | DRAFT    | $785    | 5         | Miguel      | yes          |
| JYCT_M108832 | IMPORT    | DRAFT    | $785    | 5         | Miguel      | yes          |
| JYCT_M108833 | IMPORT    | DRAFT    | $785    | 5         | Miguel      | yes          |

---

*Derivation tags: **(observed)** = read directly from this customer's billing history; **(inferred)** = one step beyond the data; **(assumed)** = carried from domain/standard practice, to confirm.*
