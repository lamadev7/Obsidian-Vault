---
type: concept
tags: [dispatcher, operations, definition]
updated: 2026-06-01
---

# What Is Dispatcher

Part of [[Dispatcher — Overview]].

## Definition

The **dispatcher** is the function (and the screen) that manages the *physical* movement of containers for a drayage carrier. It takes a customer's request to move a container and:

- tracks the container at the terminal (discharge, holds, last-free-day, availability),
- waits for the container to be **released** (customs + freight),
- assigns a **driver and truck** to the moves,
- runs the **moves** (pull from terminal → deliver to consignee → return empty),
- records arrive/depart times and milestones,
- marks the load **completed** — at which point [[Dispatcher Billing — Overview|billing]] takes over.

It does **not** handle the money. Charges, invoices and payment are the billing module's job. Dispatch ends at "completed."

## What it's responsible for

| Responsibility | Example |
|---|---|
| Know where the box is | container discharged off the vessel, sitting at the terminal |
| Know if it can move | customs + freight holds released, last-free-day not passed |
| Get a driver on it | assign driver + truck to the pull move |
| Execute the moves | pull → deliver → return, in order |
| Capture proof | geofence arrive/depart times, signed delivery |
| Close it out | mark the load completed |

## Concrete example

Load **JYCT_M108697** (real, carrier JYC Trucking, customer RT EXPRESS USA, import):

1. RT EXPRESS tenders an import container — dispatcher creates the load.
2. Dispatcher tracks it at the terminal — waits for discharge + release.
3. Once released, a driver is assigned.
4. Driver **pulls** the container from the terminal, **delivers** to the consignee, **returns** the empty.
5. Each leg's arrive/depart is logged.
6. Load marked **completed** → hand-off to [[Billing Flow|billing]].

Everything above is dispatch. The $X that RT EXPRESS later pays is [[Charge and Chargeset|billing]].

See [[Dispatcher Components]] for each piece a load carries, and [[Dispatcher Flow]] for the full lifecycle.
