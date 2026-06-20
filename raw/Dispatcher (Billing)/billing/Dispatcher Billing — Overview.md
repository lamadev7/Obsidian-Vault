---
type: overview
tags: [billing, dispatcher, charges, accounts-receivable]
updated: 2026-06-01
---

# Dispatcher Billing — Overview

How money flows on a drayage load inside PortPro, from the carrier's side. This is the **accounts-receivable (AR)** view — what the carrier bills the customer and collects.

> **Scope of this topic:** `Load → Billing → Chargeset → Charges`. Customer-side only. Driver pay and vendor/expense are referenced for contrast but are separate ledgers — see [[Charge and Chargeset]].

## The flow in one line

**Run load → add charges → carrier APPROVES the chargeset → it becomes an INVOICE → customer pays on terms → payment recorded.**

```
ops/driver adds charges
   → CARRIER clerk APPROVES chargeset      ← internal; what we want AI to do
       → INVOICE sent to customer
           → customer PAYS (Net 30 etc.)   ← carrier collects
               → payment recorded
```

## Notes in this topic

- [[Billing Flow]] — the full lifecycle, what **approve** vs **invoice** mean, when the customer pays, and the chargeStatus stages.
- [[Charge and Chargeset]] — what a charge is, what a chargeset is, the three money flows (AR / driver pay / vendor pay), charge mirroring, and the three kinds of charge (service / pass-through / penalty).
- [[Tariffs and Missing Contracts]] — tariffs as the rate source, customer vs driver tariff, and what a "missing contract" is.
- [[Charge Auto-Approval Task]] — the business goal: make AI auto-approve charges by nailing **source + calculation + approval** per charge. The single metric that matters.
- [[JYC Trucking Billing Findings]] — real data pulled from carrier `carlos@jyctrucking.com`.

## Who does what

- **Carrier billing/accounting clerk** — adds, edits, approves charges. The role AI is replacing.
- **Customer (cargo owner / broker)** — receives the invoice, pays or disputes. Never approves the carrier's charges; only accepts or pushes back.

See [[Billing Flow]] for the two distinct "approvals" people confuse (carrier internal approval vs customer acceptance).
