---
type: entity
tags: [document-validation, mongo, frontend, data-model]
sources: [[raw/DocumentValidation/Runtime Rule Firing.md]], [[raw/DocumentValidation/codes/Runtime Rule Firing Code.md]]
updated: 2026-06-02
---

# Load Doc Requirements

Mongo `load_doc_requirements` — the per-customer doc-requirement profile the **frontend** reads to render the proactive missing-doc chip + [[Step 8 Runtime Rule Fire|drop-zone]]. Written during Flow E resumption (`create_document_rule`, [[Step 5 Flow E Document Validation|Step 5]]).

## Shape (per the FE that consumes it)
Each profile carries:
- `required_data` — the doc types required (e.g. `["Proof of Delivery"]`).
- `appliedEvents` — driverOrder event ids the rule applies to (event-rule path).
- `appliedChargeSets` — charge-set ids (charge-rule path).
- `rules.is_event_rule` — which matching path applies.

## How the FE uses it
- `NewDocumentsTab` (behind `isDocRequiredAgentEnabled()`) calls `getRequireDocumentsList(loadId)` → `GET /load-doc-requirements?loadId=…` (the **Node** backend, not the AI agent) and builds a `docRulesMapper`. `NewDocumentsTab/index.js:185-225`.
- `AllUploadedDocumentsSandbox` computes `reqList` per event/charge, diffs against uploaded types → `missingRequiredDocs`, renders a `Dropzone` per missing doc. `AllUploadedDocumentsSandbox.js:90-180`. If a `DOC_REQUIREMENT` billing exception exists, that list is preferred (carries the exception id).

## vs AiRules
`load_doc_requirements` = **proactive UI** (show the chip before the rule fires). [[AiRules]] = **runtime exception** (raise `DOC_REQUIREMENT`). Both written by the same `create_document_rule` step; different audiences. Per [[Step 8 Runtime Rule Fire]].

## Related
[[Step 8 Runtime Rule Fire]] · [[Step 5 Flow E Document Validation]] · [[AiRules]] · [[Process Post AI Rules]] · [[Document Validation State]]
