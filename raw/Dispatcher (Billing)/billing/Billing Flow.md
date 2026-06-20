---
type: concept
tags: [billing, flow, invoice, approval, lifecycle]
updated: 2026-06-01
---

# Billing Flow

The lifecycle of money on one load. Part of [[Dispatcher Billing — Overview]].

## The six steps

1. **Load runs** — driver pulls the container, delivers, returns empty.
2. **Charges added** — line haul, fuel surcharge, and accessorials land on the load. See [[Charge and Chargeset]].
3. **Approve** — the carrier's clerk locks the [[Charge and Chargeset|chargeset]] as final and correct.
4. **Invoice** — the approved chargeset becomes an invoice document, sent to the customer.
5. **Customer pays** — on payment terms (e.g. Net 30), days or weeks later.
6. **Payment recorded** — invoice marked paid; loop closed.

The customer pays at **step 5** — always *after* approve (3) and *after* the invoice is sent (4). Never before.

## What "approve" means

**Approve = the carrier says "these charges are final and correct, ready to bill."**

- Locks the chargeset; no more casual edits.
- An **internal** action by the carrier's billing clerk — this is the action [[Charge Auto-Approval Task|we want AI to perform]].
- Before approve, the chargeset is still being built (draft / pending).
- The customer has seen nothing yet.

## What "invoice" means

**Invoice = the bill document created from the approved chargeset and sent to the customer.**

- Formal "you owe us $X" record.
- Carries invoice number, date, **due date**, payment terms.
- One invoice can cover one load or several loads combined.
- This is the **first** thing the customer sees.

## Approve vs Invoice

| | Approve | Invoice |
|---|---|---|
| What it is | lock charges as correct | the bill sent out |
| Who acts | carrier clerk (internal) | generated from approval, goes to customer |
| Customer sees it? | no | yes |
| Money moves? | no | not yet — starts the payment clock |

## Two different "approvals" (don't confuse)

1. **Internal approval (carrier side)** — carrier clerk approves the chargeset before billing. *This is the automation target.*
2. **Customer acceptance** — customer's AP clerk pays the invoice or disputes a line. Not something the carrier controls.

A trusted charge source (see [[Charge Auto-Approval Task]]) helps both: faster internal auto-approve **and** fewer downstream customer disputes.

## chargeStatus stages (as seen in PortPro data)

A chargeset moves through these statuses:

- **(blank)** — no charges / not started.
- **UNAPPROVED** — charges present but not yet approved.
- **BILLING** — approved / invoiced, awaiting payment.
- **PARTIAL_PAID** — some payment received.
- **FULL_PAID** — customer paid in full. Loop closed.
- **REBILLING** — invoice voided and re-issued (usually after a dispute).

Observed distribution for one real carrier in [[JYC Trucking Billing Findings]].
