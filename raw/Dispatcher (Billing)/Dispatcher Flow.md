---
type: concept
tags: [dispatcher, flow, lifecycle, routing, operations]
updated: 2026-06-01
---

# Dispatcher Flow

The operational lifecycle of a load, from creation to completion, then the hand-off to billing. Part of [[Dispatcher — Overview]]. See the pieces in [[Dispatcher Components]].

## The lifecycle

1. **Load created** — from a customer tender (manual entry, EDI, or bulk upload). Reference number assigned.
   - *Example:* RT EXPRESS tenders an import container → load `JYCT_M108697` created.
2. **Container tracked** — terminal milestones scraped: discharged, holds, last-free-day, availability.
   - *Example:* box discharged off the vessel; freight + customs holds still on.
3. **Released & ready for pickup** — holds clear (customs + freight). Only now is it dispatchable.
   - *Rule:* do not assign a driver until released — see [[Dispatcher Components|Holds & Release]].
4. **Driver assigned** — a driver + truck are put on the moves. Pre-pull may be triggered if the last-free-day is near.
5. **Moves executed** — in order, each with arrive/depart logged (often by geofence):
   - Pull container from terminal → deliver to consignee → return empty.
   - *Example:* pull from LA terminal → deliver Redlands DC → return empty to terminal.
6. **Load completed** — all moves done; load marked completed.
   - *Example:* `JYCT_M108697` completed 2026-05-20.
7. **Hand-off to billing** — charges accrue against the load and head to an invoice. Dispatch is done; the money side begins.

## Where dispatch ends and billing begins

```
[ DISPATCH ]  create → track → release → assign → moves → COMPLETED
                                                              │
                                                              ▼
[ BILLING ]   charges accrue → APPROVE chargeset → INVOICE → customer PAYS
```

The completed load is the boundary. Everything left of it is this note; everything right of it is the billing module — [[Billing Flow]] for the lifecycle, [[Charge and Chargeset]] for the pieces, [[Dispatcher Billing — Overview]] as the entry point.

## Why the dispatch detail matters to billing

Many charges get their **quantity** from dispatch events, even though their **rate** comes from a [[Tariffs and Missing Contracts|tariff]]:

- Detention hours ← arrive/depart times on the deliver move.
- Chassis rental days ← pull-to-return span.
- Storage ← last-free-day vs pickup.
- Pre-pull ← pulled early because the last-free-day was near.

That dependency is the whole reason the [[Charge Auto-Approval Task]] cares which dispatch **source** to trust.
