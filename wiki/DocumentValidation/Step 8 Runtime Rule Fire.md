---
type: source-summary
tags: [document-validation, runtime, rule-firing, drop-zone]
sources: [[raw/DocumentValidation/Runtime Rule Firing.md]], [[raw/DocumentValidation/codes/Runtime Rule Firing Code.md]]
updated: 2026-06-02
---

# Step 8 — Runtime Rule Fire

Wiki summary of [[raw/DocumentValidation/Runtime Rule Firing.md|Runtime Rule Firing]] (+ [[raw/DocumentValidation/codes/Runtime Rule Firing Code.md|code walk]]). What happens once the rule is live ([[Step 7 Customer Portal Approval]]) and a driver hits the matching event. Two halves: **(A)** the backend fires the rule on driver-arrived and raises a `DOC_REQUIREMENT` exception; **(B)** the frontend renders a red "Required: Proof of Delivery" drop-zone, and the upload runs through [[Runtime Validation Layers and Load Match|two-layer document validation]].

## Half A — rule fires (backend)
1. Driver taps "Arrived" on the DELIVERLOAD stop → mobile `PATCH tms/editTMSLoad`. `load-service.js:3977`.
2. `processPreAIRules` (filter applicable rules) → persist load → [[Process Post AI Rules|processPostAIRules]]. `load-service.js:4759`.
3. Rule filter: `module=load · customer match · status='approved' · isActive`. **No** business/engineering gate — see [[Runtime Validation Layers and Load Match]]. `ai-rule-processor/index.js`.
4. `rule-executor` runs `validation_script` → true, then `execution_script` → `publishException('DOC_REQUIREMENT')` → `billingexceptions` insert → `TOPICS.EXCEPTION.UPDATE` → Control Tower red badge. `rule-executor.js`.

## Half B — required-doc drop-zone (frontend)
5. Opening the load mounts `NewDocumentsTab`; behind the `isDocRequiredAgentEnabled()` flag it calls `getRequireDocumentsList(loadId)` and builds a `docRulesMapper` (doc type → `is_event_rule` + `appliedEvents`). `NewDocumentsTab/index.js:185-225`.
6. Backend `GET /load-doc-requirements?loadId=…` (the **Node** backend, not the AI agent) reads the [[Load Doc Requirements|load_doc_requirements]] Mongo profiles. `Billing/actionCreator.js:722-741`.
7. `AllUploadedDocumentsSandbox` computes per-event `reqList` (event path filters by `appliedEvents`; charge path by `appliedChargeSets` + `!is_event_rule`), diffs against already-uploaded types (`missingRequiredDocs`), renders a `Dropzone` per missing required doc. If a `DOC_REQUIREMENT` exception exists, that list is preferred (carries the exception id). `AllUploadedDocumentsSandbox.js:90-180`.
8. Dropping a POD → `tms/uploadDocumentForLoad` (`document-controller.js:2556`) → creates a `DocumentValidationState{PENDING}` and kicks the AI validation chain. Verdict `VALIDATED` clears the requirement; chip disappears on refresh. See [[Document Validation State]] + [[Runtime Validation Layers and Load Match]].

## Why both `load_doc_requirements` AND `AiRules`
- **[[Load Doc Requirements|load_doc_requirements]]** (Mongo) — read by the FE to show the missing-doc chip *proactively*, even before the rule fires.
- **[[AiRules|AiRules]]** (Mongo) — read by [[Process Post AI Rules|processPostAIRules]] at runtime to *raise the exception* (Control Tower / notifications).

Both written during Flow E resumption (`create_document_rule`). Different audiences: proactive UI vs runtime exception.

## Related
[[User Manual Overview]] · [[Step 7 Customer Portal Approval]] (prev) · [[Runtime Validation Layers and Load Match]] (how the upload is judged) · [[Process Post AI Rules]] · [[Document Validation State]] · [[AiRules]] · [[Load Doc Requirements]]
