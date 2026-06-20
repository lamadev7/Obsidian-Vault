---
type: concept
tags: [document-validation, picker, architecture, sse, security]
sources: [[raw/DocumentValidation/Existing Docs Picker — Build Log.md]]
updated: 2026-06-02
---

# Document Picker Architecture

How the "pick from existing documents" affordance in [[Step 5 Flow E Document Validation|Flow E Step 5]] is built across 3 repos. Per the build log [[raw/DocumentValidation/Existing Docs Picker — Build Log.md|Existing Docs Picker — Build Log]]. Session-by-session narrative: [[Flow E Existing Docs Picker Session]].

## Option C — BE proxies storage → base64
The chosen architecture: the backend reads stored docs and returns base64; the FE packs picker output into the **existing `files[]` payload** so the agent receives an **identical shape** whether the doc was freshly uploaded or picked. The [[Flow E Skill|agent/validator/skill stay unchanged]].

## Backend — `server/modules/ai-doc-picker/`
Hapi plugin, Joi validation, JWT scope `['carrier','fleetmanager','admin']`. Two endpoints:
1. `POST /tms/ai-doc-picker/list-customer-documents` — `{customerIds, docTypes, page, pageSize}` → per-doc_type buckets of past **COMPLETED + billed** loads with signed preview URLs.
2. `POST /tms/ai-doc-picker/fetch-documents-base64` — `{customerIds, documentIds}` → `{files:[{filename,mimetype,data}], skipped}`. 10 MiB/doc, 12-doc cap, MIME allowlist.

Mongo filter: `{carrier, isDeleted:{$ne:true}, caller:{$in:customerIds}, status:'COMPLETED', billingDate:{$ne:null}, 'documents.type':{$in:docTypes}}`.

## The structural SSE side-channel (the key fix)
The picker render originally depended on the LLM reproducing a long verbatim prompt — it paraphrased → sentinel-strict parser failed → no picker. Fix (mirrors `pending_sop_display`): decouple render from prose. `playbook_tools.py` stashes the suggestion to `V3ContextStore` (`pending_doc_validation_suggestion` / `pending_doc_picker_table`); `runner_v3.py` emits a structural SSE event (`v3_doc_validation_suggestion` / `doc_picker_table`); the FE attaches it to the assistant message structurally — **render no longer depends on LLM compliance.** A later **SSE-race** fix made both FE handlers *upsert* (create the placeholder if the table event arrives first), with `chat_messages.metadata` persistence as the refresh-time safety net.

## Security model (hardened in Session 3)
- **Carrier scope** — `resolveCarrierId` prefers JWT `carrierId`; both endpoints re-resolve (defence-in-depth). `fetch-documents-base64` mirrors the list filter (`caller $in + COMPLETED + billed`) so a known docId can't realize foreign documents.
- **Customer-portal reject** — `assertNotCustomerPortalUser` hard-403s `userData.isCustomer===true` (the dangerous `fleetmanager + isCustomer` subtype).
- **SSRF guard** — `isTrustedSignedUrl` https-only own-S3 allowlist (virtual-hosted + path-style); untrusted `documents[].url` → `skipped:{reason:'untrusted_url'}` (never `axios.get` an arbitrary host).

## Group-SOP fairness
Per-customer fan-out (one `$in:[cid]` query each via `Promise.all`) + `capBucketsByCustomer` at `MAX_DOCS_PER_CUSTOMER_PER_TYPE = 10`, so no single customer's recent loads crowd out another's. FE: customer dropdown (>1 customer) + per-table select-all (respects the 12-doc cap).

## Caps
- Suggestion fetch: ≤ **10 docs / type / customer**.
- Submit fetch: ≤ **12 docs**, ≤ **10 MiB / doc**, SSRF-guarded.

## Related
[[Flow E Existing Docs Picker Session]] · [[Step 5 Flow E Document Validation]] · [[Flow E Skill]] · [[SOP Embeddings]]
