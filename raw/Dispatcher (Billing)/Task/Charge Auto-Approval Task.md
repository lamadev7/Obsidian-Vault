---
type: concept
tags: [billing, automation, charges, auto-approval, playbook, task]
updated: 2026-06-01
---

# Charge Auto-Approval Task

The business goal behind studying [[Dispatcher Billing — Overview|dispatcher billing]]: make AI **auto-approve charges** so the carrier's billing clerk no longer has to. Source: leadership directive, 2026-05-31.

## The one metric

**How many charges does AI auto-approve without a human?** Everything else is secondary. The aim is to stop "throwing bodies at fixing charges" and instead spec each charge so the agent can approve it confidently.

## The three questions per charge

A charge can only auto-approve when the agent knows three things:

1. **Source** — where does the number come from, and who/what do we trust? Driver entry vs terminal/geofence vs signed TIR/PoD. *This is the hard part* — it is the dispute battleground for [[Charge and Chargeset|penalty charges]].
2. **Calculation** — the rate / formula: $/day, $/hr, % of line haul, $/mile, or pure pass-through. Comes from the [[Tariffs and Missing Contracts|tariff]].
3. **Approval** — pre-charge or post-charge? How many approvals?

Answer those three, in plain English, for every charge × every customer → the charge auto-approves.

## Worked example — Detention

- **Source:** take the hours from the **geofence** arrive/depart times, not driver-typed times. If a driver hand-edits those, hold and review.
- **Calculation:** 2 free hours, then $65/hr.
- **Approval:** post-charge, one accounting approval.

> *Detention bills when the driver is on-site past 2 free hours at the delivery stop. Use the geofence arrive/depart times, not the driver-entered times — if a driver hand-edits them, hold and review. Rate is $65/hr after the free window. One accounting approval before it goes on the invoice.*

Auto-approve test: geofence hours present, past 2 hrs, rate $65/hr → **auto-approve.** Driver hand-edited the time → **hold and flag.**

## Agent maturity — monitor the source

A naive agent just hard-calculates (2 hr × $65). A mature agent **watches the source** — is the detention source being undermined (e.g. a driver overriding the geofence)? The guide tells the agent *which source to monitor per charge*. That is the leap from reactive charge-fixing to confident auto-approval.

## Other outputs of the task

Beyond source/calc/approval, the per-customer charge guide also captures:

- **Definitions / deviations** — what each charge *means for this customer* (storage, bobtail, chassis split, detention, waiting). Flag any customer using a non-standard meaning.
- **Positive charge** — define what counts as a valid, auto-approvable charge for this customer (the baseline a clean chargeset must meet).
- **Validate against history** — list the charges actually applied historically and confirm the source + calc + approval match reality (or shadow forward for new carriers).
- **Missing-contracts list** — see [[Tariffs and Missing Contracts]]; handed to the implementation team.

## Existing carrier vs new carrier

The work mines historical [[Billing Flow|billing]] data, so the fill method differs:

- **Existing carrier (rich history)** — source, calc, approval, and history-validation are all **observable** from data. The "validate against history" step works directly.
- **New carrier (no history)** — nothing to mine. Calculation + contract coverage come from the uploaded [[Tariffs and Missing Contracts|tariffs]] (marked *assumed*); source and validation must come from onboarding interviews + **shadow-forward** (run the rules against incoming live loads). Here the guide *is* the starting playbook, not a summary of one.

> A correctness pilot should use an **existing, history-rich carrier** — a new carrier can't run the validate-against-history step.

## Deliverable shape

Per carrier → customer, free-English (no formulas):

```
<Carrier>/
  <Customer>/
    Findings/
      line-haul.md      — present? per-move or all-in? fuel folded or separate? automatable?
      contracts.md      — contracted charges vs MISSING-contract list
      accessorials.md   — every accessorial: Source / Calculation / Approval
      definitions.md    — per-charge meaning + flagged deviations
      positive-charge.md— what counts as a valid auto-approvable charge
      validate-history.md — actual charges vs derived rules; mismatches flagged
```

First real-data run: [[JYC Trucking Billing Findings]].
