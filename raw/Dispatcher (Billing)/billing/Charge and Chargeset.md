---
type: concept
tags: [billing, charge, chargeset, accounts-receivable, driver-pay]
updated: 2026-06-01
---

# Charge and Chargeset

The building blocks of [[Billing Flow]]. Part of [[Dispatcher Billing — Overview]].

## What is a charge

One **line item**. Has a name (Line Haul, Detention, Fuel Surcharge…), an amount, and a rate type ($/day, $/hr, %, flat). One charge = one billable thing on one load.

## What is a chargeset

The **bundle** of all charge lines for one load.

- Has a **status** — being built → pending → approved → billed → paid. See the stages in [[Billing Flow]].
- Has a **bill-to** — which customer gets invoiced.
- When approved and billed, it becomes an **invoice** → customer pays → payment recorded.

So: a **charge** is one line; a **chargeset** is all the lines for a load, headed to one customer's invoice.

## The three money flows (don't confuse them)

They look alike but go opposite directions.

| Flow | Who pays whom | What it's tied to |
|---|---|---|
| **Customer charges (AR)** | customer pays **carrier** | the customer (bill-to) → invoice |
| **Driver pay (settlement)** | carrier pays **driver** | the driver → settlement |
| **Vendor / expense (AP)** | carrier pays **subcarrier/vendor** | the subcarrier; only when the load is outsourced (often none — in-house) |

[[Dispatcher Billing — Overview|This topic]] is about the **AR flow** only — customer charges. Driver pay and vendor pay matter here only where they **mirror** a customer charge.

## Charge mirroring

One real-world event often creates **two** charges in opposite ledgers:

- Driver waits 3 hours → **Detention** billed to the customer (AR) **and** **detention pay** to the driver (settlement).
- Toll paid → **passed through** to the customer **and** reimbursed to the driver.

Where a customer charge has a driver-pay mirror, the mirror is a useful cross-check on the charge's source and rate.

## The three kinds of charge (real-world acceptance)

This split decides whether the customer will pay without a fight:

1. **Service** (Line Haul, Fuel Surcharge) — the work the customer hired. Always paid; no dispute.
2. **Pass-through** (Pier Pass, port fees, tolls) — costs the carrier fronted and recovers. Always paid, **but the customer wants proof** (a receipt). Carrier makes no margin here.
3. **Penalty / accessorial** (Detention, Storage, Demurrage) — paid **only if the carrier can prove it.** This is where disputes happen, and why **source** is the hard problem in [[Charge Auto-Approval Task]].

## When charges are created

- **At load creation** — Line Haul + Fuel Surcharge auto-populate from the customer's rate card. See [[Tariffs and Missing Contracts]].
- **During the lifecycle** — accessorials accrue as events happen: detention when the driver waits, storage when the last-free-day passes, chassis rental per day held, pre-pull when pulled early.
