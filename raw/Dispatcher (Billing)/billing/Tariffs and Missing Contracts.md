---
type: concept
tags: [billing, tariff, contracts, rate-card, automation]
updated: 2026-06-01
---

# Tariffs and Missing Contracts

How a charge's *rate* is decided, and what blocks automation. Part of [[Dispatcher Billing — Overview]].

## What a tariff is

A **tariff = the rate card / contract** between the carrier and a customer. It is the source of truth for *what the rate is* (the dollar figure), separate from *where the quantity comes from* (hours, days, miles — see [[Charge Auto-Approval Task]]).

Two tariff types — never mix them:

- **Customer tariff** → drives **customer charges** (the AR side, [[Charge and Chargeset]]).
- **Driver tariff** → drives **driver pay** (settlement). Separate ledger.

## How charges pull from the tariff

- Load created → the system matches load attributes (lane, container size, customer) to the customer's tariff → **auto-creates Line Haul + Fuel Surcharge** lines.
- For accessorials, the **rate** also comes from the tariff, but the **quantity** comes from the load's events (e.g. detention hours from arrive/depart times).

## Contract-backed vs manual charge

Each charge line is effectively tagged by origin:

- **From the rate card** — pulled from the tariff. Trustworthy, auto-rateable, [[Charge Auto-Approval Task|auto-approvable]].
- **Manually entered** — a person typed the amount. No tariff backing → suspect → needs review.

## Missing contracts

**A "missing contract" = a charge (or a whole customer) with no tariff to rate it from.** The clerk has to hand-type the amount every time → it can never auto-approve.

This is why the [[Charge Auto-Approval Task]] produces a **missing-contracts list** as a first-class output: it is the worklist handed to the implementation / digital-transformation teams to go fetch the agreements. Every missing contract closed = more charges that can auto-approve.
