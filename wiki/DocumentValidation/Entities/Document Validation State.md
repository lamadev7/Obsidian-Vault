---
type: entity
tags: [document-validation, runtime, state-machine, data-model]
sources: [[raw/DocumentValidation/Document Validation Overview.md]], [[raw/DocumentValidation/Runtime Rule Firing.md]]
updated: 2026-06-02
---

# Document Validation State

The runtime state machine for an uploaded document, set after `uploadDocumentForLoad`. State determination: `portpro-backend/server/modules/document/document-controller.js:1456-1471`. Per [[raw/DocumentValidation/Document Validation Overview.md|Document Validation Overview]].

## States + transitions
```
[*]            --> PENDING          : uploadDocumentForLoad
PENDING        --> VALIDATED        : load-match ok + SOP rules pass
PENDING        --> REJECTED         : Load Match fail / classification fail / clarity fail / rule failure
PENDING        --> HUMAN_REVIEW     : needs_human_review = true
PENDING        --> RETRY_EXHAUSTED  : fetch retries failed
HUMAN_REVIEW   --> VALIDATED        : manual override
HUMAN_REVIEW   --> REJECTED         : manual reject + reason
REJECTED       --> PENDING          : re-upload
RETRY_EXHAUSTED--> PENDING          : re-upload
VALIDATED      --> [*]
REJECTED       --> [*]
```

## Driven by the two validation layers
The `PENDING → VALIDATED|REJECTED` transition is decided by the doc-validator agent's [[Runtime Validation Layers and Load Match|two layers]]: built-in load-match (container/BOL/seal/booking; `reference_number` NOT matched) + customer SOP rules (`v2RulesCompliance`). `VALIDATED` clears the `DOC_REQUIREMENT` exception; rejections carry an `issueType` (Load Match Issue / clarity / rule failure).

## Related
[[Step 8 Runtime Rule Fire]] · [[Runtime Validation Layers and Load Match]] · [[Load Doc Requirements]] · [[Process Post AI Rules]]
