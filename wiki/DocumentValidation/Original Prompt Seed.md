---
type: source-summary
tags: [document-validation, seed, requirements]
sources: [[raw/DocumentValidation/Prompt.md]]
updated: 2026-06-02
---

# Original Prompt Seed

The informal ask that seeded the whole [[User Manual Overview|Document Validation flow]]. Per [[raw/DocumentValidation/Prompt.md|Prompt]]. It describes the intended user journey for creating document-validation rules for a specific customer.

## What it asked for

An 8-point flow: portal → AI Agent Hub → Customer SOPs → create SOP for a customer → prompt the agent in chat (`"Make POD required on driver arrved on deliver events"`) → agent builds a stepper plan (add playbook content, verify entities, review playbook text, show preview, set test loads, save playbook, process new scenarios) → pick test loads → upload sample example documents → admin approval → customer-portal playbook approval → runtime firing with a file-upload drop-zone on the matching event.

## Divergence from the live implementation

The seed prompt described a **three-button admin approval** (`View Rules → Approve`, then **Business Approved**, then **Engineering Approved**). The live code collapsed this to a **single gate** — see [[Approval State Gate]] and [[Step 6a Admin Approve Rules]]. The business/engineering buttons still render but are vestigial; their flags aren't consumed by the live rule-firing path ([[Runtime Validation Layers and Load Match]]).

The seed also asked, of Step 8: *"might trigger ai generated rules, find out if yes then share ref code link"* — answered in [[Process Post AI Rules]] and [[Step 8 Runtime Rule Fire]]: yes, `processPostAIRules` fires the AI-authored [[AiRules|AiRule]] and raises a `DOC_REQUIREMENT` exception.

## Related
[[User Manual Overview]] (the structure this seeded) · [[Step 6a Admin Approve Rules]] · [[Approval State Gate]]
