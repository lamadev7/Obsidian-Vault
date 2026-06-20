---
type: concept
tags: [document-validation, runtime, rule-firing, backend]
sources: [[raw/DocumentValidation/Runtime Rule Firing.md]], [[raw/DocumentValidation/codes/Runtime Rule Firing Code.md]]
updated: 2026-06-02
---

# Process Post AI Rules

The backend chain that fires an AI rule at runtime. Per [[raw/DocumentValidation/Runtime Rule Firing.md|Runtime Rule Firing]] (+ [[raw/DocumentValidation/codes/Runtime Rule Firing Code.md|code walk]]). Runs when a driver taps "Arrived" → mobile `PATCH tms/editTMSLoad`.

## The chain
```
load-service.js:3977  processPreAIRules(LOAD, [UPDATE], ctx)   // filter applicable rules
                      → persist load
load-service.js:4759  processPostAIRules(LOAD, [UPDATE], ctx)
   └─ ai-rule-processor/index.js : filter module=load · customer match · status='approved' · isActive
   └─ rule-executor.js : validation_script(ctx)=true → execution_script
   └─ publishException('DOC_REQUIREMENT')
   └─ billingexceptions insert → TOPICS.EXCEPTION.UPDATE → Control Tower red badge
```

## The gate
The rule filter is `module=load · customer match · status='approved' · isActive` — plus an active [[Playbook Scenarios|playbook_scenarios]] row. **No** business/engineering gate (`engineering_approved`/`business_approved` are not consulted). Full conditions: [[Approval State Gate]]. Why those columns are excluded: [[Runtime Validation Layers and Load Match]].

## Pre vs Post
- `processPreAIRules` — filters which rules apply, runs before persist.
- `processPostAIRules` — selects approved+active rules for the customer and runs the `validation_script` → `execution_script` chain after persist. This is the one that raises `DOC_REQUIREMENT`.

Refs: `ai-rule-processor/index.js`, `rule-executor.js`, `DOC_REQUIREMENT` in `portpro-mcp/.../exception.constant.ts`.

## Related
[[Step 8 Runtime Rule Fire]] · [[Approval State Gate]] · [[Runtime Validation Layers and Load Match]] · [[AiRules]] · [[Playbook Scenarios]]
