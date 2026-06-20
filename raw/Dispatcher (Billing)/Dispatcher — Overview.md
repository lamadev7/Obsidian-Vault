---
type: overview
tags: [dispatcher, operations, drayage, tms]
updated: 2026-06-01
---

# Dispatcher — Overview

The **dispatcher** is the operations side of the drayage TMS — it turns a customer's container-move request into executed truck moves, and drives each load from the port to the door and back. **Billing** (the money side) is one module inside it.

> **Mental split:** Dispatch = the *physical move* (assign drivers, track containers, run the moves). Billing = the *money* for that move. Dispatch finishes when the load is **completed**; billing takes over from there.

## Notes in this topic

- [[What Is Dispatcher]] — definition, what it's responsible for, a concrete example.
- [[Dispatcher Components]] — every piece a load carries (order, customer, parties, equipment, routing/moves, driver, appointments, holds, tracking, documents, billing) with an example each.
- [[Dispatcher Flow]] — the operational lifecycle from load creation to completion, then the hand-off to billing.

## The billing module (linked, not duplicated)

Everything about charges, chargesets, invoices, tariffs and payment lives in the **billing** submodule:

- **Entry point:** [[Dispatcher Billing — Overview]]
- Key pages: [[Billing Flow]] · [[Charge and Chargeset]] · [[Tariffs and Missing Contracts]] · [[Charge Auto-Approval Task]] · [[JYC Trucking Billing Findings]]

The hand-off point — load **completed** → charges accrue → chargeset approved → invoice → payment — is described in [[Dispatcher Flow]] and detailed in [[Billing Flow]].

## One-line picture

**Customer tenders a container → dispatcher tracks it, releases it, assigns a driver, runs the moves (pull → deliver → return) → load completed → [[Dispatcher Billing — Overview|billing]] takes over.**
