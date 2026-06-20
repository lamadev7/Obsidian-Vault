---
type: source-summary
tags: [document-validation, flow-e, picker, security, session-log]
sources: [[raw/DocumentValidation/Existing Docs Picker — Build Log.md]]
updated: 2026-06-02
---

# Flow E Existing Docs Picker Session

Wiki summary of the 4-session build log [[raw/DocumentValidation/Existing Docs Picker — Build Log.md|Existing Docs Picker — Build Log]]. The work that added the **"pick from existing documents"** affordance to [[Step 5 Flow E Document Validation|Flow E Step 5]] across **3 repos** (`portpro-frontends`, `portpro-backend`, `portpro-ai-agents`), branch `feat/flow-e-existing-docs-picker`. Architecture detail lives in [[Document Picker Architecture]].

## Goal
Let a dispatcher pick existing uploaded docs from past **COMPLETED + billed** loads of the same customer, instead of (or alongside) a fresh upload. **Option C** chosen: BE proxies storage → base64; FE packs picker output into the existing `files[]` payload so the agent receives an **identical shape** regardless of provenance.

## Session 1 (2026-05-26) — design + code analysis
- **New BE plugin** `server/modules/ai-doc-picker/` (Hapi, Joi, JWT scope `['carrier','fleetmanager','admin']`). Two endpoints: `POST /tms/ai-doc-picker/list-customer-documents` and `POST /tms/ai-doc-picker/fetch-documents-base64`.
- Mongo filter: `{carrier, isDeleted:{$ne:true}, caller:customerId, status:'COMPLETED', billingDate:{$ne:null}, 'documents.type':{$in:docTypes}}`.
- **ai-agents** copy-only edit to `playbook_tools.py` (STOP_INSTRUCTION wording "upload" → "provide"); FE adds an inline `DocumentPicker` module + `parseDocValidationSuggestion` prose parser.

## Session 2 (2026-05-26) — picker not rendering → structural side-channel
**Bug:** picker render depended on the LLM faithfully reproducing a long verbatim prompt; it paraphrased → sentinel-strict parser → `present:false` → no picker. **Fix:** decouple render from LLM prose via a **structural SSE event** (`v3_doc_validation_suggestion`), mirroring the proven `pending_sop_display` pattern — `playbook_tools.py` stashes the suggestion to `V3ContextStore`, `runner_v3.py` emits the event, FE attaches it structurally. Detail: [[Document Picker Architecture]].

## Session 3 (2026-05-27) — audit re-verify + **security remediation**
Re-audited 8 findings; three real security bugs **confirmed + fixed**:
- **Cross-customer fetch** — `fetch-documents-base64` filter lacked `caller`/`status`/`billingDate`; any fleetmanager (incl. customer-portal subtype) with a known docId could realize foreign documents as base64. Fixed: filter mirrors the list filter.
- **SSRF** — `bucket-service.js` short-circuited and returned user-controlled `documents[].url` unchanged; `axios.get` could pivot to an arbitrary host. Fixed: `isTrustedSignedUrl` https-only own-S3 allowlist; untrusted → `skipped:{reason:'untrusted_url'}`.
- **Customer-portal user** — `fleetmanager + isCustomer=true` is the dangerous subtype. Fixed: `assertNotCustomerPortalUser` hard 403 on `isCustomer===true`.
- Also: **group-SOP single-customer bug** (`customer_id` scalar → `customer_ids: List[str]`), stale `last_doc_picker_args` (clear on upload + scenario start), and a **dead-code purge** (deleted the original `DocumentPickerCard/Modal/Column` + 2 dead utils; the live chain is `DocumentPickerTable` + `doc_picker_table` SSE).
- **Picker SSE race** (patch): `doc_picker_table` arrives before the assistant placeholder exists → silently dropped; refresh masked it (BE persists to `chat_messages.metadata`). Fixed with an **upsert** in both SSE handlers + `streamStarted=true`.

## Session 4 (2026-05-28) — group-SOP fairness + UX
- **Fairness bug:** a single global `billingDate DESC` over-fetch let one customer's recent loads crowd out the other's in a group SOP. **Fix:** per-customer fan-out (`Promise.all`, one `$in:[cid]` query each) + `capBucketsByCustomer` → `MAX_DOCS_PER_CUSTOMER_PER_TYPE = 10`; pagination no-op'd (picker is single-shot). Worst case 100 rows/type (10 customers × 10).
- **Second pass (FE-only):** Customer column (shown only on multi-customer "All" view) + per-table select-all header checkbox (respects the 12-row global cap, `indeterminate` via ref).

## Caps (final)
- **Suggestion fetch cap:** ≤ **10 documents per type per customer** (`MAX_DOCS_PER_CUSTOMER_PER_TYPE`) — bounds the universe.
- **No selection cap on the user** in the picker list, but submit `fetch-documents-base64` caps at **12 docs** (`DOC_PICKER_MAX_SELECT`) + **10 MiB/doc**, SSRF-guarded to the own-S3 allowlist.

## Known blockers / follow-ups
- **FE jest BLOCKED** (pre-existing on PRODUCTION-DRAYOS): `jest@30` vs transitively-resolved `jest-environment-jsdom@27.5.1` → `Cannot read properties of undefined (reading 'html')`. Fix exists on `feature/disputes-ship` (`02e6f27d23a`), unmerged. **Push held** until that lands (avoids bundling ~1.4k lines of lockfile churn).
- BE `customer_labels` join not yet populated → dropdown may show raw ObjectIds.
- Detector policy still misses event-triggered "doc required" prose without acceptance criteria (Conv A class) — separate decision.
- Reload persistence to `chat_messages.metadata` for the suggestion is a follow-up.

## Related
[[Document Picker Architecture]] · [[Step 5 Flow E Document Validation]] · [[Flow E Skill]] · [[SOP Embeddings]] · [[User Manual Overview]]
