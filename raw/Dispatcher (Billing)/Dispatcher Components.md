---
type: concept
tags: [dispatcher, components, load, routing, equipment]
updated: 2026-06-01
---

# Dispatcher Components

The pieces a load carries on the dispatch side. Part of [[Dispatcher — Overview]]. Examples use real fields from carrier JYC Trucking.

## 1. Load / Order

The core unit of work — one container move. Carries a reference number and a load type.

- **Example:** `JYCT_M108697`, type **import**. Reference prefix encodes the carrier (`JYCT_`).

## 2. Customer (Caller)

Who tendered the load and who gets billed (the bill-to).

- **Example:** RT EXPRESS USA, ARTIS SOLUTIONS LLC, BEST BAY LOGISTICS. The customer owns the cargo, not the trucks. → drives [[Charge and Chargeset|billing]].

## 3. Parties & Locations

- **Shipper** — for an import, the pickup terminal/port; for an export, the origin.
- **Consignee** — the delivery destination (warehouse / DC).
- **Empty origin / return** — where the empty container goes back.
- **Example:** import pulled from the LA terminal, delivered to a Redlands DC, empty returned to the terminal.

## 4. Equipment

- **Container** — size, type, owner (e.g. 40ft, owned by the steamship line).
- **Chassis** — the wheeled frame; its owner and size. Often rented → a [[Charge and Chargeset|chargeable]] item.
- **Example:** 40ft container MSCU1234567 on a pool chassis.

## 5. Routing / Moves (Driver Order)

The ordered sequence of events the driver executes. Each move has an arrive and a depart.

- **Example sequence (import):** Pull Container (arrive/depart terminal) → Deliver Load (arrive/depart consignee) → Return Container (arrive/depart terminal).
- Arrive/depart times are the **source** for time-based charges like detention — see [[Charge Auto-Approval Task]].

## 6. Driver & Truck

The assigned driver and equipment that execute the moves. A load can be split across drivers (one pulls/drops, another hooks/returns).

- **Example:** Driver A pulls and drops at the yard; Driver B hooks and returns the empty.

## 7. Appointments

Booked time windows at the terminal or consignee for pickup / delivery / return.

- **Example:** terminal return appointment booked for a specific window; missing it can trigger fees.

## 8. Holds & Release

A container can't be dispatched until holds clear.

- **Holds:** broker, customs, freight, other.
- **Rule:** do not assign a driver until **customs + freight are released**. See [[Dispatcher Flow]].

## 9. Tracking / Status

Container milestones, often scraped from the terminal portal: discharged date, outgate, ingate, last-free-day (LFD), vessel.

- **Example:** LFD approaching → trigger a pre-pull to avoid storage. The LFD is also the **source** for a storage/demurrage charge.

## 10. Documents

Proof captured during the move: TIR (in/out), Proof of Delivery (PoD), Bill of Lading (BoL).

- **Example:** signed PoD proves delivery and is the **source** for confirming a delivered date; a TIR proves a chassis split. Feeds document-driven charges in [[Charge Auto-Approval Task]].

## 11. Billing module

Once the load is completed, charges accrue and head to an invoice. This is its own submodule — start at [[Dispatcher Billing — Overview]].

- **Example:** line haul + fuel surcharge + chassis rental + detention → chargeset → approved → invoice → customer pays. Full detail in [[Billing Flow]] and [[Charge and Chargeset]].
