# Session — Flow E Existing-Docs Picker — Code Analysis

**Date:** 2026-05-26
**Branch (all 3 repos):** `feat/flow-e-existing-docs-picker`
**Worktree root:** `/Users/bikram/Documents/portpro/.worktrees/flow-e-existing-docs/`
**Tracked QA report:** `.deepagents/states/qa-reports/flow-e-existing-docs.md`

---

## Goal

Add a second affordance to Doc Validation user-manual Step 5 (Flow E). Dispatcher can pick existing uploaded docs from past COMPLETED + billed loads of the same customer, instead of (or alongside) a fresh upload. Selected docs flow through the same Flow E pipeline — agent/validator/skill stay unchanged.

Architecture (Option C — chosen): BE proxies storage → base64. FE packs picker output into existing `files[]` payload, so agent receives identical shape regardless of provenance.

---

## Repo Branch Status

| Repo | Branch | Tracking | Working tree |
|------|--------|----------|--------------|
| `portpro-frontends` | `feat/flow-e-existing-docs-picker` | `origin/PRODUCTION-DRAYOS` (up-to-date) | dirty — picker code untracked, 2 modified files + 1 stray CSS |
| `portpro-backend`   | `feat/flow-e-existing-docs-picker` | `origin/PRODUCTION-DRAYOS` (up-to-date) | dirty — module untracked, `manifest.js` + `package-lock.json` modified |
| `portpro-ai-agents` | `feat/flow-e-existing-docs-picker` | `origin/PRODUCTION` (up-to-date) | dirty — `playbook_tools.py` modified |

No commits ahead of base in any repo yet. Nothing pushed. No PRs.

---

## portpro-backend — `server/modules/ai-doc-picker/`

NEW plugin. Hapi.js, Joi validation, JWT auth scope `['carrier', 'fleetmanager', 'admin']`.

### Files

| File | Lines | Role |
|------|-------|------|
| `index.js` | 117 | Hapi routes + Joi schema |
| `ai-doc-picker-controller.js` | 239 | Carrier-scoped Mongo query + base64 fetch logic |
| `ai-doc-picker-utils.js` | 119 | Pure helpers — filename/MIME extract, filter builder, bucket grouping, paging, concurrency |
| `ai-doc-picker-constants.js` | 38 | Page sizes, MAX 10MB/doc, MAX 12/fetch, allowed MIME, skip reasons |

Modified: `server/manifest.js` — plugin registered right after `document/index`.

### Endpoints

1. **`POST /tms/ai-doc-picker/list-customer-documents`**
   - Payload: `{ customerId, docTypes[1..10], page>=1, pageSize<=25 }`
   - Returns `{ columns: { [docType]: { documents[], page, pageSize, totalCount, hasMore } } }`
   - Each `documents[]` row: `{ documentId, loadId, loadReferenceNumber, filename, mimetype, previewUrl, uploadedAt, billingDate, partyName, hasSignature }`

2. **`POST /tms/ai-doc-picker/fetch-documents-base64`**
   - Payload: `{ documentIds[1..12] }`
   - Returns `{ files: [{filename, mimetype, data}], skipped: [{documentId, reason}] }`
   - `data` is base64 (NOT data-URL prefixed).

### Carrier scoping (key rule check)

`controller.js:34-39` — `resolveCarrierId(userData)`:
- Prefer `userData.carrierId` from JWT.
- Fallback to `EditLoadService.getUserDetailV1({ _id })`.
- Throws "Carrier context missing" when both miss. ✅ Matches CLAUDE.md rule.

### Mongo query shape

`utils.js:31-38` — `buildCompletedBilledLoadFilter`:
```js
{
  carrier: carrierId,
  isDeleted: { $ne: true },
  caller: customerId,
  status: 'COMPLETED',
  billingDate: { $ne: null },
  'documents.type': { $in: docTypes },
}
```
- ✅ `carrier` scoped
- ✅ `isDeleted: { $ne: true }`
- Filters to COMPLETED + billed loads only (billed = `billingDate != null`)
- `caller` is the customer ObjectId on the load doc

### Listing flow

`controller.js:41-142`:
1. Build filter (carrier + customer + COMPLETED + billed + type-in-array).
2. Over-fetch `min(pageSize × docTypes × 5, 500)` loads sorted by `billingDate` desc — single Mongo round-trip, projection trimmed to `{reference_number, documents, billingDate}`.
3. Group `loads[].documents[]` into per-type buckets in JS (`collectDocumentsByType`).
4. Per-doc filters: skip `doc.isDeleted` ✅, skip if `doc.type` not in requested list.
5. Sort each bucket by `uploadedAt || billingDate` desc.
6. Paginate slice per type.
7. Sign preview URLs concurrently (4 in parallel, failures swallowed → `previewUrl=null`).

### Base64 fetch flow

`controller.js:144-232`:
1. Re-resolve carrier from JWT (defence-in-depth — second auth check).
2. Second Mongo query, carrier-scoped, `documents._id ∈ ids`.
3. Build `Map<docIdStr, docSubDoc>`.
4. Per doc, concurrency 4:
   - MIME allowlist check → skip with `unsupported_mimetype` if fail.
   - **Re-sign URL** at fetch time (fresh signed URL, defeats stale URL bug).
   - `axios.get` with `responseType: 'arraybuffer'`, 30s timeout, `maxContentLength = MAX + 1`.
   - Buffer length check vs `MAX_BYTES_PER_DOC` (10 MB) → skip with `exceeds_max_size`.
   - Base64 encode, push to `files[]`.
5. Failures recorded with reason, not thrown.

### Concerns / Notes

- **No total-row global cap**: each `documents[]` array on a load is unbounded; if a customer has loads with massive doc arrays, `collectDocumentsByType` does in-memory work proportional to `overFetchLimit × avg-docs-per-load`. Cap (500 loads × maybe 20 docs) ≈ 10k — fine.
- **`paginateBucket` over a pre-fetched window**: pagination is over the over-fetched 500-load window, not the full dataset. Past page 50 or so, dispatcher hits the ceiling. Acceptable for an MVP since dispatcher only needs a handful.
- **`partyName`, `hasSignature` projected on doc** — these are not standard on every `documents[]` schema variant. Surface as nullable; no error if missing.
- **No `customerGroup` support** in BE list endpoint. FE modal has a customer dropdown for group SOPs and switches `activeCustomerId`, but only single-customer queries reach the BE. ✅ Matches Option C plan — group SOPs require manual customer selection in modal.

### Tests

`tests/unit/modules/ai-doc-picker/`:
- `ai-doc-picker-utils.test.js` — 20 tests
- `ai-doc-picker-controller.test.js` — 16 tests

Per QA report: all 36 passing.

---

## portpro-ai-agents — `app/agents/billing_agent_v2/playbook_tools.py`

Single file, copy-only change. No behavior change.

### Two symmetric edits

1. **`save_playbook`** (~line 797) — `STOP_INSTRUCTION` after detecting `document_validation_suggestion`.
2. **`update_playbook`** (~line 1612) — same instruction at edit-playbook path.

### What changed

Both sites changed in identical ways:

- "upload" → "provide" in the directive verbs (so agent doesn't pre-commit user to upload-only).
- Prompt text extended: `… please upload one or two VALID example documents per type, **or choose from existing documents on past completed loads for this customer**. Reply "skip" if you want to do this later.`
- Added clarification to agent: `When samples arrive (either uploaded fresh or selected from past loads — the file payload is identical in both cases), pivot to the flow-e-document-validation skill`.

### Parity with FE parser

FE `parseDocValidationSuggestion.js` sentinel:
```
"choose from existing documents on past completed loads"
```
Matches both new prompt strings exactly (lowercased compare). ✅

Both regex variants still match — `SAVE_REGEX` keys on `"Your playbook mentions rules for these documents: <X>. To make the AI validator"`, which is unchanged. `UPDATE_REGEX` keys on `"Your edit affects document rules for: <X>. Please upload one or two VALID"`, also unchanged at the head.

### Risk

Agent instruction is prose, not structured. LLM may paraphrase verbatim prompts in edge cases — historically the orchestrator follows STOP_INSTRUCTION literally per the `Use this exact prompt:` directive, so risk is low. FE parser is fail-open (`present:false` returns silently) — non-Flow-E chats unaffected.

---

## portpro-frontends — `src/pages/tms/AIHub/AIChatV2/`

### New: `Components/DocumentPicker/`

| File | Role |
|------|------|
| `constants.js` | Default page size 10, max select 12, auto-prompt copy, skip-reason → user-copy map |
| `documentPickerActionCreators.js` | Thin HTTP wrappers around both BE endpoints, JWT auth header |
| `useDocumentPicker.js` | Per-doctype columns state, selection set (Set), paging, submit |
| `DocumentPickerCard.jsx` | Inline chat card with "Choose from existing documents" CTA |
| `DocumentPickerColumn.jsx` | Per-type column inside modal — checkbox list + Load more |
| `DocumentPickerModal.jsx` | Bootstrap modal `bsSize="xl"`, kanban of columns, source-customer dropdown for group SOPs |
| `__tests__/useDocumentPicker.test.js` | 10 unit tests |

### New: `utils/parseDocValidationSuggestion.js` (+ tests)

Regex parser for assistant chat content. Returns `{ present, docTypes[], variant: 'save' | 'update' | null }`. Sentinel-gated so generic mentions don't accidentally trigger.

### Modified: `Components/ChatEmptyState.jsx`

- Imports `DocumentPickerCard`, `DOC_PICKER_AUTO_PROMPT`, `parseDocValidationSuggestion`.
- `docValidationSuggestion` memo: scans messages from newest, finds latest assistant message, parses. Returns first parse result.
- `pickerCustomerIds` memo: prefers `selectedCustomer.originalData.customer_ids` (group SOP) over `customerId`. `null` for generic playbooks (no customer scope).
- `handlePickerSelection({files})` → calls existing `handleSendMessage(DOC_PICKER_AUTO_PROMPT, …, files)`. Picker output reuses chat send-path verbatim.
- Renders `<DocumentPickerCard>` between `ChatContainer` and `ChatInput`, only when suggestion present AND picker customer IDs exist.

### Modified: `hooks/useSopChatMessages.js`

Only `processFiles` changed (`:32-62`). Per-entry shape guard:
- If entry is object with `typeof data === 'string'` AND `typeof filename === 'string'` → pass through `{filename, mimetype || 'application/octet-stream', data}`.
- Otherwise (native `File`) → legacy `fileToBase64` path.

Native `File` object lacks `data` and `filename` props (it has `name` not `filename`), so shape check cleanly bisects the two ingestion paths.

### Unrelated change in working tree

`src/assets/css/login.css` — diff is comment shuffle + missing source map line. Looks like SCSS rebuild artifact, NOT a deliberate edit. Should revert before commit.

---

## Cascade Verdict

Per CLAUDE.md cascade rule:

- **`processFiles`** — sole caller is internal `sendMessage` in same hook. No exports. Guard shape-driven. Native `File` path preserved. ✅
- **`DBLoadQueryService.find`** — used widely. No model/schema change here. ✅
- **`BucketService.getSignedURL`** — used widely. No-op on existing callers. ✅
- **Route paths `/tms/ai-doc-picker/*`** — grep confirms no collisions.
- **`document_validation_suggestion` payload** — surfaced in agent tool response but previously unparsed in FE. New parser fail-open; zero risk to non-Flow-E.

Across **3 repos**, **modified surfaces are minimal** (1 file each except FE, which adds an isolated module + 2 small mods). New code is additive; nothing rewires existing flows.

---

## Open Items

1. **Revert** `src/assets/css/login.css` (stray build artifact, unrelated).
2. **Commit + push** all three worktrees with `<identifier>:` prefix.
3. **Open PRs** against:
   - `portpro-frontends` → `PRODUCTION-DRAYOS` (default base)
   - `portpro-backend` → `PRODUCTION-DRAYOS`
   - `portpro-ai-agents` → `PRODUCTION`
4. **Manual staging E2E** — 10 scenarios listed in QA report.
5. **Optional follow-up**: MCP-tool parity wrapper. Skipped per plan.

---

## File Pointers (worktree-absolute)

```
BE:  /Users/bikram/Documents/portpro/.worktrees/flow-e-existing-docs/portpro-backend/server/modules/ai-doc-picker/
BE:  …/portpro-backend/tests/unit/modules/ai-doc-picker/
BE:  …/portpro-backend/server/manifest.js (modified)
FE:  /Users/bikram/Documents/portpro/.worktrees/flow-e-existing-docs/portpro-frontends/src/pages/tms/AIHub/AIChatV2/Components/DocumentPicker/
FE:  …/portpro-frontends/src/pages/tms/AIHub/AIChatV2/utils/parseDocValidationSuggestion.js
FE:  …/portpro-frontends/src/pages/tms/AIHub/AIChatV2/utils/__tests__/parseDocValidationSuggestion.test.js
FE:  …/portpro-frontends/src/pages/tms/AIHub/AIChatV2/Components/ChatEmptyState.jsx (modified)
FE:  …/portpro-frontends/src/pages/tms/AIHub/AIChatV2/hooks/useSopChatMessages.js (modified)
AG:  /Users/bikram/Documents/portpro/.worktrees/flow-e-existing-docs/portpro-ai-agents/app/agents/billing_agent_v2/playbook_tools.py (modified)
```

---

# Session 2 — 2026-05-26 — Picker Not Rendering: Debug + Structural Fix

## Bug Report (from staging conversation)

Two failing conversations:

**Conv A (save flow, fast path)** — playbook: *"required POD on driver arrived on deliver event"*. Agent went preview → test loads → save → fast-path `create_document_rule`. **No picker appeared.** No `"choose from existing documents on past completed loads"` sentinel emitted.

**Conv B (update flow)** — playbook updated to require POD with signature. Agent emitted heavily-paraphrased prompt:
> "📄 Your playbook now includes document validation rules… You can either: Upload a sample document here in the chat, Choose from past loads that have valid PODs"

User replied "Choose from past loads" → agent fell into chat path asking which load ref. **No picker.**

## Root-Cause Analysis

| Conv | Cause |
|------|-------|
| A | Detector returned `has_document_requirements=false` because prose lacked acceptance criteria (`prompts.py` "be conservative — NOT validation: generic mention without acceptance criteria"). `suggestion=None` → STOP_INSTRUCTION not overridden → no sentinel → no picker. **Pre-existing detector policy**, not introduced by branch. |
| B | Detector fired (signature criterion). STOP_INSTRUCTION override fired. But LLM **paraphrased** the verbatim "Use this exact prompt:" directive. `parseDocValidationSuggestion` is sentinel-strict → `present:false` → picker not rendered. |

Branch diff confirmed copy-only inside STOP_INSTRUCTION strings — gate/detector/fast-path logic was **pre-existing**.

Common root: **picker render depended on LLM faithfully reproducing a long natural-language sentence.** Unreliable.

## Fix — Structural Side-Channel via SSE Event

Decouple picker render from LLM prose entirely. Same pattern as `pending_sop_display` in `runner_v3.py`.

### Architecture map (via Explore agent)

- `runner_v3.py:3991` `append_event()` is single emit point; persists to Redis (`AgentRunEventStoreRedis`), polled by `ai_chat_v2.py:6488` `/chat/runs/{run_id}/events` (300ms poll). No filtering — all events pass through to FE.
- Existing event types: `v3_text`, `v3_sop`, `v3_todo`, `v3_feedback`.
- Existing tool→runner channel: `V3ContextStore.set_value(session.id, "pending_sop_display", {...})` → runner reads + emits `v3_sop` → clears key. Two emit blocks: in-loop (after each agent step) + final (last-tool-call cases).
- `chat_messages.metadata` JSONB column exists but not used in flow E reload path yet.

### Wiring (additive only, mirrors `pending_sop_display`)

**`portpro-ai-agents/app/agents/billing_agent_v2/playbook_tools.py`**:
- `save_playbook` line 826-838: after `response["document_validation_suggestion"] = suggestion`, stash same dict via `V3ContextStore.set_value(session_id, "pending_doc_validation_suggestion", suggestion)`. Wrapped in try/except — Redis hiccup never breaks save.
- `update_playbook` line 1618-1630: same.

**`portpro-ai-agents/app/agents/billing_agent_v2/sop/runner_v3.py`**:
- In-loop block (5001-5021) + final block (5057-5077): read `pending_doc_validation_suggestion` → emit SSE event:
  ```json
  {"type":"document_validation_suggestion","data":{"action":"create","doc_types":["Proof of Delivery"],"parent_sop_id":"...","parent_customer_ids":[...],"existing_doc_sop_id":null,...}}
  ```
  → `event_type="v3_doc_validation_suggestion"` → clear key.

**`portpro-frontends/src/pages/tms/AIHub/AIChatV2/utils/docValidationSuggestion.js`** (NEW):
- Pure adapter `adaptDocValidationSuggestion(raw)` — snake_case wire keys → camelCase shape matching DocumentPickerCard contract + prose-parser output.
- Output: `{present, docTypes, variant ('save'|'update'), source:'structural', action, parentSopId, parentCustomerIds, existingDocSopId}`.
- Defensive: null/non-object/empty doc_types → returns `null`.

**`portpro-frontends/src/pages/tms/AIHub/AIChatV2/hooks/useSopChatMessages.js`**:
- Import adapter (line 9).
- `sendMessage` SSE handler (241-265): on `event.type === 'document_validation_suggestion'` → adapt → attach to assistant message by `aiMessageId` as `docValidationSuggestion`.
- `processStreamingResponse` resume path (713-728): same, by `messageId` or last-assistant fallback.

**`portpro-frontends/src/pages/tms/AIHub/AIChatV2/Components/ChatEmptyState.jsx`** memo (75-90):
- Prefer `message.docValidationSuggestion` (structural) → fall back to `parseDocValidationSuggestion(content)` (prose parser).
- Old reloaded conversations still work via fallback.

### Why this fixes both bugs

- **Conv A**: still requires detector to flag prose. Bug stays (separate detector-policy issue). Documented as follow-up — pure event-triggered "doc required" prose without acceptance criteria still misses. Not a regression introduced by branch.
- **Conv B**: detector fires → suggestion built → V3ContextStore stash → runner emits SSE event regardless of LLM text → FE attaches structurally → picker renders even if LLM paraphrases. **Fixed.**

## Tests

| Layer | File | Status |
|---|---|---|
| FE adapter | `utils/__tests__/docValidationSuggestion.test.js` NEW | 17 cases — happy, defensive, contract parity |
| FE prose parser (fallback) | `utils/__tests__/parseDocValidationSuggestion.test.js` | 10 cases — unchanged, still passing |
| FE picker hook | `Components/DocumentPicker/__tests__/useDocumentPicker.test.js` | 10 cases — unchanged |
| FE smoke | `__tests__/pages/tms/AIHub/AIChatV2/Components/AIChatV2Agents.test.jsx` | unchanged |
| BE Python | none added | runner emit block is exact mirror of proven `pending_sop_display`; `ast.parse` clean on both modified files |

```
Test Suites: 4 passed, 4 total
Tests:       28 passed, 28 total
```

## Regression Analysis

| Surface | Risk | Reasoning |
|---|---|---|
| Existing SSE event types | None | New branch added via `else if` after existing branches |
| Reloaded historical convos | None | Memo falls back to prose parser → identical to pre-change |
| `<DocumentPickerCard>` / `<DocumentPickerModal>` | None | Props contract unchanged |
| Non-Flow-E chats | None | Structural event only emitted when stash key non-empty; stash only set when detector returns suggestion |
| BE save flow | None | Stash wrapped in try/except |
| Runner emit blocks | None | Mirror proven pattern; clear-after-emit prevents replay |
| LLM compliance | Eliminated as failure mode | Render decoupled from agent prose |

## End-to-End User Flow (post-fix)

1. User authors playbook with doc-validation prose.
2. Agent calls `save_playbook` / `update_playbook` → detector flags → suggestion built + stashed.
3. Runner emits structural SSE event.
4. FE attaches `docValidationSuggestion` to assistant message.
5. `<DocumentPickerCard>` renders below ChatContainer.
6. User clicks **Choose from existing documents** → modal opens.
7. Per doc_type column, 10 docs from past COMPLETED+billed loads, multi-select up to 12.
8. Submit → `fetch-documents-base64` → `files[]` → `handleSendMessage(DOC_PICKER_AUTO_PROMPT, ..., files)`.
9. Agent receives identical-shape payload as fresh upload → pivots to flow-e-document-validation skill.
10. Next assistant turn lacks `docValidationSuggestion` → picker hides.

## Open Follow-ups (post-merge)

1. **Detector policy** — loosen `playbook_doc_detector/prompts.py` if event-triggered "doc required" prose (no acceptance criteria) should also trigger picker. Currently misses Conv A class — separate decision (risk: more false positives).
2. **Reload persistence** — persist `document_validation_suggestion` to `chat_messages.metadata` JSONB so picker survives page reload mid-flow. Currently ephemeral.
3. **Python tests** — none added; runner block is mirror of proven pattern. Add focused integration test if/when test harness for `runner_v3` exists.

## File Changes (Session 2)

```
BE:  …/portpro-ai-agents/app/agents/billing_agent_v2/playbook_tools.py (modified — 2 stash blocks)
BE:  …/portpro-ai-agents/app/agents/billing_agent_v2/sop/runner_v3.py (modified — 2 emit blocks)
FE:  …/portpro-frontends/src/pages/tms/AIHub/AIChatV2/utils/docValidationSuggestion.js (NEW — adapter)
FE:  …/portpro-frontends/src/pages/tms/AIHub/AIChatV2/utils/__tests__/docValidationSuggestion.test.js (NEW — 17 tests)
FE:  …/portpro-frontends/src/pages/tms/AIHub/AIChatV2/hooks/useSopChatMessages.js (modified — SSE handlers in 2 paths)
FE:  …/portpro-frontends/src/pages/tms/AIHub/AIChatV2/Components/ChatEmptyState.jsx (modified — memo)
```

---

# Session 3 — 2026-05-27 — Audit re-verify + blocker remediation

**Operator:** Claude Code (CAVEMAN mode, full).
**Trigger:** "re-analyzed the changes in all current local changes, and reverify the above issues, then we will move for imporvement and refractors".
**Branch (all 3 repos):** `feat/flow-e-existing-docs-picker` — same worktree, no rebases.

## Re-Audit Verdict Matrix

All 8 prior Session-2 findings re-checked against current code in worktree. None flipped.

| # | Issue | Location | Verdict |
|---|-------|----------|---------|
| 1 | Silent sort drop | `ai-doc-picker-controller.js:92-97` | CONFIRMED. `DBLoadQueryService.find` ignores `options.sort` (the 4th-arg sort key is never extracted); the chained `.sort(sortBy)` at line 40 of `db-load-query-service.js` is the only honored sort, defaulting to `{_id:-1}` when the 6th positional `sortBy` arg is omitted. |
| 2 | Cross-customer fetch | `ai-doc-picker-controller.js:165-169` | CONFIRMED. Filter was `{carrier, isDeleted, 'documents._id': $in}` — no caller / status / billingDate. Any fleetmanager (incl. customer-portal subtype) with a known docId could realize foreign documents as base64. |
| 3 | SSRF | `bucket-service.js:42` + `controller:208-214` | CONFIRMED. `if (!filePath?.includes(s3BucketName)) return filePath;` short-circuits and returns user-controlled URL unchanged. `Load.documents[].url = payload.url` verbatim (`document-controller.js:2137`). Attacker pivots `axios.get` into arbitrary host. |
| 4 | FE dead-code | `AIChatV2/Components/DocumentPicker/` Card/Modal/Column/useDocumentPicker + `utils/parseDoc…` + `utils/docValidationSuggestion` + SSE branches in `useSopChatMessages.js` | CONFIRMED. `msg.docValidationSuggestion` written in three places, read in zero. Live chain: `DocumentPickerTable` + `doc_picker_table` SSE + `constants.DOC_PICKER_AUTO_PROMPT` (consumed by `ChatEmptyState`). |
| 5 | login.css noise | `src/assets/css/login.css` | CONFIRMED. Formatter artifact — comment moved one block down, trailing `sourceMappingURL` dropped. Zero semantic change. |
| 6 | Group-SOP single customer | `playbook_tools.py:2960` + `picker_args_for_stash` builders L857/L1673 | CONFIRMED. Tool signature `customer_id: str`. Both stash builders take `parent_customer_ids[0]` only — every group-SOP picker fire was scoped to the first customer alone. |
| 7 | `last_doc_picker_args` stale | `playbook_tools.py:876, 1695` | CONFIRMED. Source comment: "Cleared only on explicit skip or when a new playbook save overwrites it." File-upload / scenario-start did not clear → intent regex (`\blist documents\b`, etc.) could re-fire picker on stale state in a later turn. |
| 8 | `resp.text[:200]` URL echo | `playbook_tools.py:3023` | LOW RISK CONFIRMED. BE error path is `appUtilityFunctions.sendError(err.message)` — unlikely to embed URLs absent regression. Worth scrubbing defensively. |

Fresh notes added during re-audit:

| # | Note | Location |
|---|------|----------|
| 9 | resolveCarrierId role exception list | `controller:34-46` — excludes {driver, fleetmanager, customer, yardmanager}. Any new role falls through to `_id` path. Validated against `getUserDetailV1` comment; acceptable while role set is stable. |
| 10 | Route auth scope | `index.js:48, 90` — `['carrier','fleetmanager','admin']`. `fleetmanager + isCustomer=true` (customer-portal route, set in `organization-controller.js:287/342/400`) IS the dangerous subtype. |

## Decisions

**Group-SOP path = (a) — customer dropdown on the live `DocumentPickerTable`.** Single picker fire across the group. BE accepts `customerIds[]`, returns per-doc `customerId`. ai-agents tool takes `customer_ids: List[str]`. `picker_args_for_stash` carries full list. UX: dropdown shown only when >1 customer (default `__ALL__`); single-customer SOPs unchanged.

**Auth scope (#10) = controller-side reject of `userData.isCustomer === true`** rather than narrowing the JWT scope list (would break legitimate fleetmanager ops users who run Flow E).

## Remediation Implemented

### portpro-backend

- `ai-doc-picker-constants.js`
  - Added `MAX_CUSTOMER_IDS = 10`, `MAX_DOC_TYPES = 10`, `SKIP_REASONS.UNTRUSTED_URL = 'untrusted_url'`.
- `ai-doc-picker-utils.js`
  - `buildCompletedBilledLoadFilter({ carrierId, customerIds, docTypes })` → `caller: { $in: customerIds }`.
  - `collectDocumentsByType` propagates `load.caller` → per-doc `customerId` (needed for FE dropdown filter).
  - NEW `isTrustedSignedUrl(urlString, bucketName)` — https-only hostname allowlist for own S3 bucket: virtual-hosted-style (`<bkt>.s3.amazonaws.com`, `<bkt>.s3.<region>.amazonaws.com`, `<bkt>.s3-<region>.amazonaws.com`) + path-style (`s3-<region>.amazonaws.com/<bkt>/…`).
- `ai-doc-picker-controller.js`
  - `assertNotCustomerPortalUser(userData)` — hard 403 reject for `userData.isCustomer === true`.
  - `normalizeCustomerIds(raw)` — coerces, dedupes, caps at `MAX_CUSTOMER_IDS`, returns ObjectId list.
  - `listCustomerDocuments` accepts `customerIds`, projects `caller: 1`, uses 6th-positional `sortBy = { billingDate: -1 }` (the actual honored sort), returns `{ success, customer_ids, columns }` with each doc carrying `customerId`.
  - `fetchDocumentsAsBase64` accepts `customerIds`, mirrors list filter (`caller $in + status COMPLETED + billingDate $ne null`), SSRF-guards `axios.get` with `isTrustedSignedUrl` against the per-server-bound `s3BucketName` (read from `server.plugins['core-config'].S3bucketConfig.config.s3BucketCredentials.bucket`). Untrusted URL → `skipped: [{ reason: 'untrusted_url' }]`.
- `index.js`
  - `buildUserData` now captures `isCustomer: !!session.isCustomer`.
  - Both routes: Joi payload swapped `customerId: string` → `customerIds: array(string).min(1).max(MAX_CUSTOMER_IDS)`. Auth scope unchanged.

### portpro-ai-agents

- `playbook_tools.py`
  - Added `_URL_SCRUB_PATTERN` + `_scrub_urls(text)` helper.
  - `fetch_existing_customer_documents(customer_ids: List[str], doc_types, …)` — accepts list, tolerates stray scalar for backwards-compat, posts `{customerIds, docTypes}` to BE, scrubs URLs from `resp.text` before truncating to 200 chars, returns `customer_ids` in payload, stashes `{customer_ids, doc_types, columns}` to `pending_doc_picker_table`. STOP_INSTRUCTION wording adapted for "1 customer" / "N customers" cases.
  - Both `picker_args_for_stash` builders (save_playbook at L857; update_playbook at L1673) now write `customer_ids: list(parent_customer_ids or [])` instead of `customer_id: parent_customer_ids[0]`. Updated comments mark that the long-lived `last_doc_picker_args` clears on file upload + scenario start (new behavior).
  - Both STOP_INSTRUCTION strings updated to render `fetch_existing_customer_documents(customer_ids=[…], doc_types=[…])` in the branching-rules prose.
- `sop/runner_v3.py`
  - Picker intercept (`if not is_resume and not files and message:` ~L4856) — added `_picker_args_complete(d)` + `_picker_customer_ids(d)` helpers that tolerate both the new `customer_ids` list shape AND the legacy `customer_id` scalar shape (so an in-flight session straddling the schema flip doesn't lose intercept).
  - Tool call swapped: `fetch_existing_customer_documents(customer_ids=intercept_customer_ids, …)`.
  - File upload path (~L5179) — after stashing `uploaded_files`, clears both `awaiting_doc_picker_choice` and `last_doc_picker_args` so a later text reply matching the intent regex cannot re-fire the picker on stale state.
  - `compile_playbook_scenarios` entry — same dual clear of picker args. Scenario compilation is past the Flow-E sample-collection step; lingering picker intent is always stale at this boundary.

### portpro-frontends

- Files deleted (dead chain):
  - `AIChatV2/Components/DocumentPicker/DocumentPickerCard.jsx`
  - `AIChatV2/Components/DocumentPicker/DocumentPickerModal.jsx`
  - `AIChatV2/Components/DocumentPicker/DocumentPickerColumn.jsx`
  - `AIChatV2/Components/DocumentPicker/useDocumentPicker.js`
  - `AIChatV2/Components/DocumentPicker/__tests__/useDocumentPicker.test.js` (+ empty dir removed)
  - `AIChatV2/utils/parseDocValidationSuggestion.js`
  - `AIChatV2/utils/docValidationSuggestion.js`
  - `AIChatV2/utils/__tests__/docValidationSuggestion.test.js`
  - `AIChatV2/utils/__tests__/parseDocValidationSuggestion.test.js` (+ empty dir removed)
- `AIChatV2/hooks/useSopChatMessages.js`
  - Dropped `adaptDocValidationSuggestion` import.
  - Removed two `event.type === 'document_validation_suggestion'` SSE branches (L241–L264 and L747–L762 of pre-edit file) that wrote `msg.docValidationSuggestion` (never read).
- `AIChatV2/Components/DocumentPicker/documentPickerActionCreators.js`
  - Removed dead `listExistingCustomerDocuments`.
  - `fetchSelectedDocumentsAsBase64({ customerIds, documentIds })` — both required.
- `AIChatV2/Components/DocumentPicker/DocumentPickerTable.jsx`
  - Reads `data.customer_ids` (falls back to `data.customer_id` then doc-discovered set) and `data.customer_labels`.
  - `selectedCustomerId` state defaults to `__ALL__` for multi-customer payloads, single customer otherwise.
  - `filteredColumns` memo filters per bucket by `selectedCustomerId`.
  - Dropdown header rendered only when >1 customer (`<select>` with All + per-customer options, disabled while submitting / submitted).
  - `handleSubmit` posts `{ customerIds, documentIds }` to the BE fetch endpoint.
  - PropTypes extended with `customer_ids` + `customer_labels`.
- `src/assets/css/login.css` — reverted (was pure formatter noise from earlier session).

## Test Suite

| Repo | Suites | Tests | Result |
|------|--------|-------|--------|
| `portpro-backend` (`tests/unit/modules/ai-doc-picker`) | 2 | 51 | PASS — covers customerIds $in filter, sort-via-6th-arg assertion, dedupe + cap, SSRF allowlist (7 cases incl. metadata-IP block), customer-portal hard reject on both routes, fetch-base64 mirrored filter assertion. |
| `portpro-ai-agents` (`py_compile` × 4 files) | n/a | n/a | CLEAN |
| `portpro-frontends` (`AIHub\|AIChat\|SopChat\|sop` pattern) | 9 | 67 | PASS — no jsdom symlink failures this session (the Session-2 3/3 FE failure does not reproduce). |

## Dead-Ref Grep

```
grep -rn "adaptDocValidationSuggestion|parseDocValidationSuggestion|DocumentPickerCard|DocumentPickerModal|DocumentPickerColumn|useDocumentPicker|listExistingCustomerDocuments|docValidationSuggestion" src/
```
→ zero hits across `portpro-frontends/src`.

## Risk / Caveats

- BE `customer_labels` not yet populated. Dropdown currently shows raw customer IDs for >1-customer SOPs. Cheap follow-up: BE `list-customer-documents` joins Customer doc and adds `customer_labels: { [id]: name }`. Out of scope for this audit-remediation pass.
- SSRF guard rejects (skipped: `untrusted_url`) but does NOT alert. If any legitimate prod load has a non-S3 `documents[].url`, that doc becomes unfetchable via picker — surfaced as a per-doc skip in the response. Worth a low-noise log monitor.
- `compile_playbook_scenarios` clear is bracket-defensive — only fires when that tool runs. A user who bails out at scenario-review without compiling will still have stale `last_doc_picker_args` until next save. Acceptable; the intent-regex bug only fires under a very specific re-trigger sequence anyway.

## File Changes (Session 3)

```
BE:  ai-doc-picker-constants.js (modified — 3 added consts/reasons)
BE:  ai-doc-picker-utils.js (modified — customerIds $in + per-doc customerId + isTrustedSignedUrl)
BE:  ai-doc-picker-controller.js (modified — major rewrite: customerIds + sort-fix + filter-mirror + SSRF guard + customer-portal reject)
BE:  ai-doc-picker/index.js (modified — Joi customerIds[], buildUserData captures isCustomer)
BE:  tests/unit/modules/ai-doc-picker/ai-doc-picker-utils.test.js (modified — new buildFilter shape + isTrustedSignedUrl 7 cases + customerId propagation)
BE:  tests/unit/modules/ai-doc-picker/ai-doc-picker-controller.test.js (rewritten — core-config plugin, trusted-shaped signed URL helper, SSRF skip test, customer-portal reject test, filter mirror test)

AI:  playbook_tools.py (modified — fetch tool sig + _scrub_urls + STOP wording + 2 picker_args_for_stash builders)
AI:  sop/runner_v3.py (modified — dual-shape picker_args helpers + dual clear on upload + dual clear on compile_playbook_scenarios)

FE:  DELETED Card/Modal/Column/useDocumentPicker + 2 dead utils + 3 dead test files
FE:  useSopChatMessages.js (modified — drop adaptDocValidationSuggestion import + 2 dead SSE branches)
FE:  DocumentPicker/documentPickerActionCreators.js (modified — drop listExisting…, fetchSelected… now requires customerIds)
FE:  DocumentPicker/DocumentPickerTable.jsx (modified — customer_ids[] + customer dropdown + filteredColumns memo + customerIds in submit)
FE:  src/assets/css/login.css (reverted to base)
```

## Next Steps

1. Diff-review gate + `git add` only the intended scope. NO `package-lock.json` noise — check whether the BE `package-lock.json` change is genuine (new dep?) or stray and revert if stray.
2. Commit per repo following PortPro commit conventions (Conventional Commits, domain scope, ticket-id-less branch — no Jira on this work yet).
3. Open PRs against `PRODUCTION-DRAYOS` (BE + FE) and `PRODUCTION` (ai-agents).
4. Soft follow-up later: BE `customer_labels` join + low-noise `UNTRUSTED_URL` skip log alert.

---

## Session 3 Patch — Picker SSE Race (live-render bug)

**Reported (screenshot):** User typed "2. show existing". Assistant ack rendered ("Fetching past completed-load documents for this customer…"). Picker table did NOT render inline. Page refresh → table appears. Repro: every picker-intercept fire.

**Root cause:** SSE event order from picker-intercept on backend.

`runner_v3.py` picker-intercept block emits (in order):
1. `doc_picker_table` event (~L5025) — pulled from `pending_doc_picker_table` stash.
2. `text` ack event (~L5095) — the "Fetching past…" prose.
3. `done` (~L5110) — terminal.

FE's `useSopChatMessages.js` had two `doc_picker_table` SSE handlers (one per code path — `processStreamingResponse` L240, `sendMessage` L722). Both used the same shape:

```js
setMessages((prev) => prev.map((msg) =>
  msg.id === targetId ? { ...msg, docPickerTable: tableData } : msg
));
```

Problem: target assistant placeholder is created lazily inside `updateMessage()`, which runs only on the first `text` SSE event. On the first SSE packet (`doc_picker_table`) no `msg.id === targetId` exists → `prev.map(...)` returns the array unchanged → table payload silently dropped.

Refresh masked the bug because BE persists the table to `chat_messages.metadata.doc_picker_table` (set in `runner_v3.py` ~L5149) and the `loadMessages` rehydrate path reads it back at `useSopChatMessages.js:343` (`persistedDocPickerTable`).

**Fix:** Upsert pattern in both SSE handlers.

`processStreamingResponse` (path 1):

```js
} else if (event.type === 'doc_picker_table' && event.data) {
  const tableData = event.data;
  setMessages((prev) => {
    const targetIdx = appendToExisting
      ? (prev.length > 0 && prev[prev.length - 1].role === 'assistant'
          ? prev.length - 1
          : -1)
      : prev.findIndex((m) => m.id === messageId);
    if (targetIdx >= 0) {
      return prev.map((msg, idx) =>
        idx === targetIdx ? { ...msg, docPickerTable: tableData } : msg
      );
    }
    return [
      ...prev,
      {
        id: messageId,
        content: accumulatedText,
        role: 'assistant',
        created_at: new Date().toISOString(),
        isStreaming: true,
        todos: currentTodos,
        docPickerTable: tableData,
      },
    ];
  });
  streamStarted = true;
}
```

`sendMessage` (path 2) — mirrored shape against `aiMessageId`. Same logic — find by id, patch if exists, else push placeholder carrying the table.

Critical detail: setting `streamStarted = true` after the upsert. When the subsequent `text` event arrives, `updateMessage()` checks `streamStarted` and enters the else-branch (patch by id) instead of pushing a duplicate placeholder. Net effect: one assistant message, table attached from the first packet, text accumulated into the same message as it streams.

**Regression test added (DocumentPickerTable.test.jsx):**

| # | Case |
|---|------|
| 1 | renders dropdown when `customer_ids` has >1 entry, default `All (N)` |
| 2 | hides dropdown when single customer |
| 3 | dropdown filters docs by selected customerId |
| 4 | uses `customer_labels` lookup in option text |
| 5 | submit posts full `customerIds` list + selected `documentIds` |
| 6 | tolerates legacy `customer_id` scalar payload (back-compat) |
| 7 | derives customer set from doc rows when top-level field absent |

7/7 PASS.

**Full regression matrix (post-patch):**

| Repo | Suites | Tests | Result |
|------|--------|-------|--------|
| `portpro-backend` (`tests/unit/modules/ai-doc-picker`) | 2 | 51 | PASS |
| `portpro-ai-agents` (py_compile × 4) | n/a | n/a | CLEAN |
| `portpro-frontends` (`AIHub\|AIChat\|SopChat\|sop\|DocumentPicker`) | **10** | **74** | PASS |

Files touched (Session-3 patch):
```
FE:  AIChatV2/hooks/useSopChatMessages.js (modified — upsert race guard in both SSE handlers)
FE:  __tests__/pages/tms/AIHub/AIChatV2/Components/DocumentPickerTable.test.jsx (NEW — 7 cases)
```

Note for next session: the BE-side persistence path (`runner_v3.py` ~L5149) is preserved. Refresh rehydrate keeps working as the safety net. The upsert is the LIVE render path; both must work together — losing either degrades UX.

---

## Session 4 — 2026-05-28 — Group-SOP customer fairness in picker list endpoint

**PM report:** "Group SOP shows docs for only one customer; even with 2 customers in the group, the picker is biased — I only see one customer's documents."

### Symptom verified via code review

`portpro-backend/server/modules/ai-doc-picker/ai-doc-picker-controller.js:115-143` (Session-3 form):

```js
const overFetchLimit = Math.min(
  safePageSize * normalizedDocTypes.length * customerObjectIds.length * 5,
  1000
);
const loads = await services.DBLoadQueryService.find(
  filter,                                  // caller: { $in: [A, B] }
  projection, [], { limit: overFetchLimit },
  false, { billingDate: -1 }              // GLOBAL sort across customers
);
const buckets = collectDocumentsByType({ loads, docTypes });
// then paginateBucket(bucket, page=1, pageSize=10)
```

Math at defaults (pageSize=10, 2 doc_types, 2 customers): `overFetchLimit = 10×2×2×5 = 200`. If Customer A had ≥200 recent COMPLETED+billed loads, every fetched row belonged to A (sort by `billingDate DESC` pulled A's recent loads first); Customer B contributed zero. Even when B's rows entered the pool, `paginateBucket` sliced top-10 of a date-sorted bucket — A's recency continued to dominate page 1.

Independent of the global cap, a doc_type bucket re-sorted by `uploadedAt` DESC also biased toward whichever customer had the most recent docs of that type. Result: per-doc_type bucket page 1 frequently showed 10 A docs + 0 B docs whenever A had 10+ recent docs of that type.

### Design alignment with PM

Confirmed UX intent on 2026-05-28: one table per doc_type, customer dropdown above all tables filtering rows in-place. BE returns ≤ N docs per (customer, doc_type) so the dropdown's per-customer view never goes blank for a customer that has matching docs in storage.

`MAX_DOCS_PER_CUSTOMER_PER_TYPE = 10` chosen — picker is single-shot, not paginated. Max bucket size = 10 × N_customers per type, bounded by `MAX_CUSTOMER_IDS = 10` so worst case = 100 rows per type.

### Patch (uncommitted on worktree)

**BE — `portpro-backend/server/modules/ai-doc-picker/`:**

1. `ai-doc-picker-constants.js` — new `MAX_DOCS_PER_CUSTOMER_PER_TYPE = 10`.

2. `ai-doc-picker-utils.js` — new helper `capBucketsByCustomer(buckets, perCustomerCap)`:

```js
const capBucketsByCustomer = (buckets, perCustomerCap) => {
  if (!buckets || typeof perCustomerCap !== 'number' || perCustomerCap < 1) {
    return buckets || {};
  }
  const out = {};
  for (const [docType, docs] of Object.entries(buckets)) {
    if (!Array.isArray(docs) || docs.length === 0) { out[docType] = []; continue; }
    const perCustomer = new Map();
    for (const d of docs) {
      const cid = d?.customerId || '__none__';
      if (!perCustomer.has(cid)) perCustomer.set(cid, []);
      const arr = perCustomer.get(cid);
      if (arr.length < perCustomerCap) arr.push(d); // input already date-DESC sorted
    }
    const merged = [];
    for (const arr of perCustomer.values()) merged.push(...arr);
    merged.sort((a, b) => {
      const aT = new Date(a.uploadedAt || a.billingDate || 0).getTime();
      const bT = new Date(b.uploadedAt || b.billingDate || 0).getTime();
      return bT - aT;
    });
    out[docType] = merged;
  }
  return out;
};
```

Input from `collectDocumentsByType` is already date-DESC per type; keeping first N per customer = most-recent N per customer. Final re-sort gives the "All customers" view a unified date-DESC ordering.

`paginateBucket` kept exported for back-compat; no longer called in the controller.

3. `ai-doc-picker-controller.js` — `listCustomerDocuments` rewritten:

```js
const perCustomerOverFetch = Math.min(
  MAX_DOCS_PER_CUSTOMER_PER_TYPE * normalizedDocTypes.length * 5,
  500
);

const loadsByCustomer = await Promise.all(
  customerObjectIds.map((cid) =>
    services.DBLoadQueryService.find(
      buildCompletedBilledLoadFilter({
        carrierId,
        customerIds: [cid],            // single-customer per query
        docTypes: normalizedDocTypes,
      }),
      projection,
      [],
      { limit: perCustomerOverFetch }, // per-customer budget
      false,
      { billingDate: -1 }              // 6th-positional sort honored
    )
  )
);
const loads = loadsByCustomer.flat();

const rawBuckets = collectDocumentsByType({ loads, docTypes: normalizedDocTypes });
const cappedBuckets = capBucketsByCustomer(rawBuckets, MAX_DOCS_PER_CUSTOMER_PER_TYPE);

for (const docType of normalizedDocTypes) {
  const bucket = cappedBuckets[docType] || [];
  // mapConcurrent → signed URLs for the entire capped bucket (no paginateBucket slice)
  // ...
  columns[docType] = {
    documents: finalDocs,
    totalCount: finalDocs.length,
    hasMore: false,
    perCustomerCap: MAX_DOCS_PER_CUSTOMER_PER_TYPE,
  };
}
```

`page` / `pageSize` payload params accepted (Joi schema unchanged, ai-agents tool's `pageSize: 20` still validates) but no-op'd at the controller level — destructured into `_page` / `_pageSize`.

**FE — `portpro-frontends/.../DocumentPicker/`:**

- `constants.js` — one-line addition: `SKIP_REASON_COPY.untrusted_url: "could not be verified as a trusted document URL"`. Closes Session-3 gap where BE added `UNTRUSTED_URL` reason but FE friendly-copy map didn't.
- `DocumentPickerTable.jsx` — **NO CHANGES**. Existing customer dropdown (`:200-218`, rendered only when `customerIds.length > 1`), existing `filteredColumns` memo (`:133-144`, filters by `d.customerId === selectedCustomerId`), existing tables-per-doc_type render loop (`:232-328`), and existing submit pipeline that passes the full `customerIds` set (NOT the dropdown filter) all already implement the requested UX.
- `DocumentPickerTable.test.jsx` — 3 new cases:
  - Filter to C2 hides C1 rows and shows empty-state for a doc_type bucket with no C2 rows.
  - Dropdown filter survives submit — `fetchSelectedDocumentsAsBase64` still receives `customerIds: [C1, C2]` even after filtering to C2 only.
  - `untrusted_url` skip surfaces friendly toastr copy.

**ai-agents — NO CHANGES.** Tool contract (`fetch_existing_customer_documents`) was already correct from Session 3 — accepts `customer_ids: List[str]`, posts `customerIds` to BE. Bug was purely BE-side fairness.

### Why FE didn't need code changes

The picker UX described by PM ("number of tables based on doc_types, customer filter above tables, picking one customer filters all tables to that customer's data") matched the Session-3 FE implementation 1:1. The change was BE returning data shaped right for the existing FE.

`DocumentPickerTable.jsx`:
- `:69-72` — `docTypes` derives from `data.doc_types`, one render iteration per type.
- `:77-90` — `customerIds` memo prefers `data.customer_ids` (Session-3 schema), falls back to legacy `customer_id` scalar, then discovers from doc rows.
- `:94-96` — dropdown default = `__ALL__` when >1 customer, single customer otherwise.
- `:133-144` — `filteredColumns` per-bucket filter by `selectedCustomerId`.
- `:164-167` — submit always carries the full `customerIds` set (not the dropdown filter) so BE re-scope filter still trusts the original auth boundary.

### Tests

| Layer | Suite | Tests | Result |
|-------|-------|-------|--------|
| BE | `tests/unit/modules/ai-doc-picker` | 63 (12 NEW) | ✅ PASS |
| ai-agents | `py_compile playbook_tools.py runner_v3.py` | — | ✅ CLEAN |
| FE | `@babel/parser` syntax + JSX parse on touched files | 2 | ✅ OK |
| FE | jest runtime sweep (AIHub\|AIChat\|SopChat\|sop\|DocumentPicker) | — | ⚠️ BLOCKED |

**FE jest runtime is BLOCKED pre-existing on PRODUCTION-DRAYOS.** Root cause: `package.json` declares `jest@^30.2.0` but no `jest-environment-jsdom` dependency — transitive resolution pulls `jest-environment-jsdom@27.5.1` which is incompatible with jest@30 (v27 reads `config.testEnvironmentOptions.html` which jest@30 no longer guarantees). Every jsdom-needing FE suite fails at suite-load with:

```
TypeError: Cannot read properties of undefined (reading 'html')
  at new JSDOMEnvironment (node_modules/jest-environment-jsdom/build/index.js:72:44)
```

Affects ALL 10 jsdom suites currently in `AIHub|AIChat|SopChat|sop|DocumentPicker` scope, not just the picker. Fix exists on `origin/feature/disputes-ship` commit `02e6f27d23a` ("fix(test): repair jest-environment-jsdom — pin v30 + cleanup jest-dom imports") which adds `jest-environment-jsdom@^30.2.0` to devDeps + removes broken `@testing-library/jest-dom/extend-expect` imports from 5 test files. NOT yet merged into PRODUCTION-DRAYOS.

### New BE jest coverage (`ai-doc-picker-controller.test.js` + `ai-doc-picker-utils.test.js`)

| # | Case | Layer |
|---|------|-------|
| 1 | Fan-out: 2 customers → 2 finds, each `$in` length 1, all carrier-scoped | controller |
| 2 | Per-customer cap: A has 15 PODs + B has 2 → returns 10 A + 2 B, `perCustomerCap=10` echoed | controller |
| 3 | Doc_type with zero rows from customer B doesn't fabricate B in that column | controller |
| 4 | Back-compat: `page=1, pageSize=20` payload accepted, output still capped to 10 per customer | controller |
| 5 | Dedupe customerIds: 3 input → 2 unique fan-out calls | controller |
| 6 | `capBucketsByCustomer` caps each customer slice at perCustomerCap | utils |
| 7 | Merged bucket re-sorted date DESC for "All" view | utils |
| 8 | Preserves docs from every requested customer (50 A + 2 B → 10 A + 2 B) | utils |
| 9 | doc_type a customer has zero rows for isn't faked | utils |
| 10 | Docs without customerId fall under `__none__` group, still capped | utils |
| 11 | Empty buckets pass through | utils |
| 12 | Invalid cap (0, -1, null) returns input unchanged (defensive) | utils |

### Status

Patch is on worktree `.worktrees/flow-e-existing-docs/` across portpro-backend + portpro-frontends, layered on top of Session-3 commit `cc8742505a`. UNCOMMITTED.

**Holding push until `feature/disputes-ship` env fix merges into PRODUCTION-DRAYOS.** Decision rationale: bundling the env fix into this branch would add ~1.4k lines of lockfile churn + adjacent test-file edits unrelated to the picker fairness fix — clean scope wins, even at the cost of a wait.

**Resume order (next session):**

1. `git log origin/PRODUCTION-DRAYOS --oneline | grep -i jest-environment-jsdom` — confirm env fix landed.
2. From worktree, `git pull --rebase origin PRODUCTION-DRAYOS` on `feat/flow-e-existing-docs-picker` in each repo.
3. Re-run BE jest (expect 63/63), ai-agents py_compile, FE jest `AIHub|AIChat|SopChat|sop|DocumentPicker` (expect green this time).
4. Continue at Session-3 step 1 (BE `package-lock.json` spot-check, then commits, then PRs to PRODUCTION-DRAYOS for BE+FE, PRODUCTION for ai-agents).

---

## Session 4 second pass — 2026-05-28 — picker UX upgrades (Customer column + select-all checkbox)

PM screenshot 2026-05-28 + request: (1) when dropdown = `All`, show which customer each row belongs to via a dedicated column; (2) per-table header checkbox to bulk-select every row in that bucket (respecting the 12-row global cap).

### FE-only change — `DocumentPickerTable.jsx`

**Refactor:** extracted per-bucket render into a `DocPickerBucket` subcomponent. Each bucket now owns its own `headerCheckboxRef`. Cleaner than a `useRef` map keyed by docType in the parent — no ref leakage, no map invalidation concerns when filter changes.

**Customer column logic:**

```js
const showCustomerColumn =
  showCustomerDropdown && selectedCustomerId === ALL_CUSTOMERS;
```

- `showCustomerDropdown` is the existing flag for `customer_ids.length > 1`.
- Column is shown only when both: (a) multi-customer SOP AND (b) user is on the merged "All customers" view. Filtering to a single customer hides the column — every row would otherwise show the same customer label (visual noise).
- Single-customer SOPs never show the column (no dropdown either).

Cell content uses the `customer_labels` map from the BE response, with a 3-tier fallback:

```js
const customerLabel =
  (customerLabels && customerLabels[doc.customerId]) ||
  doc.customerId ||
  "—";
```

If a customer_label join missed (e.g. BE `CustomerModel.find` threw and was swallowed, leaving `customer_labels: {}` — see Session 3), the raw ObjectId still renders. Never empty.

Column inserted between `Load` and `Billing date`:

```
| ☐ | File | Load | Customer | Billing date | Preview |
```

`<th>` width = 140px, `<td>` `text-truncate` with full label as `title=` attribute.

**Select-all header checkbox:**

Per-bucket state derived inside `DocPickerBucket`:

```js
const bucketIds = useMemo(() => docs.map(d => d.documentId), [docs]);
const bucketSelectedCount = bucketIds.reduce(
  (n, id) => (selectedIds.has(id) ? n + 1 : n), 0);
const allSelected = bucketIds.length > 0 && bucketSelectedCount === bucketIds.length;
const someSelected = bucketSelectedCount > 0 && !allSelected;
```

Cap enforcement in `toggleBucket`:

```js
if (allSelected) {
  bucketIds.forEach(id => next.delete(id));
} else {
  for (const id of bucketIds) {
    if (next.size >= DOC_PICKER_MAX_SELECT) break;  // 12
    next.add(id);
  }
}
```

Scenarios:
- Bucket has 8 PODs, 8 SCALEs (total 16): click POD select-all → 8 selected. Click SCALE select-all → adds 4 more (cap hit), stops. SCALE header shows `indeterminate` because 4 of its 8 rows are selected.
- Bucket has 3 rows, click select-all → all 3 added. Header `checked=true`.
- Click again on allSelected header → removes all 3 of bucket's IDs from `selectedIds` (won't touch other buckets' selections).
- Bucket empty (filter excluded all rows) → empty-state UI renders instead of `<table>`; header doesn't exist.

**`indeterminate` plumbing:** React doesn't model checkbox indeterminate as a prop. Driven via ref + `useEffect`:

```jsx
const headerCheckboxRef = useRef(null);
useEffect(() => {
  if (headerCheckboxRef.current) {
    headerCheckboxRef.current.indeterminate = someSelected;
  }
}, [someSelected]);

<input ref={headerCheckboxRef} type="checkbox" ... />
```

Standard React pattern for this DOM-property-only flag.

**Header disabled gates:**

```js
const headerDisabled = submitted || isSubmitting || bucketIds.length === 0;
```

Prevents re-selection during inflight submit, prevents post-submit changes (button already says "Attached"), and avoids ghost-click on an empty bucket.

### Tests — `DocumentPickerTable.test.jsx` (11 new cases, 17 total)

New `describe` blocks:

**Customer column (group SOP, All view)** — 4 cases:
- Column renders when group SOP AND dropdown=All.
- Column hidden when filtered to single customer.
- Column hidden for single-customer SOP.
- Cell falls back to customerId when label missing.

**Select-all header checkbox** — 7 cases:
- Toggles all rows in bucket only (not cross-bucket).
- Click on `allSelected` deselects all in bucket.
- Cross-bucket cap respected: 8 POD select-all + 8 SCALE select-all stops at 12.
- `indeterminate` true when partial selection.
- `checked=true` when all-selected.
- Header disabled / absent when bucket empty after filter.

### Tests run

- **BE jest** — 63 PASS / 0 fail / 2 suites. No regression from no-BE-changes-this-pass; sanity sweep.
- **FE @babel/parser** — both `DocumentPickerTable.jsx` + `DocumentPickerTable.test.jsx` parse clean (sourceType:module, plugins:[jsx]).
- **FE jest runtime** — still BLOCKED by the same pre-existing PRODUCTION-DRAYOS env breakage (jest@30 + jest-environment-jsdom@27 mismatch + babel `loose` mode plugin conflict). Disputes-ship fix `02e6f27d23a` still unmerged. Resume order in earlier Session-4 block applies.

### Worktree state after second pass

```
portpro-backend:
  M server/modules/ai-doc-picker/ai-doc-picker-constants.js     (+ MAX_DOCS_PER_CUSTOMER_PER_TYPE)
  M server/modules/ai-doc-picker/ai-doc-picker-controller.js    (per-customer fan-out + cap)
  M server/modules/ai-doc-picker/ai-doc-picker-utils.js         (+ capBucketsByCustomer)
  M tests/unit/modules/ai-doc-picker/ai-doc-picker-controller.test.js
  M tests/unit/modules/ai-doc-picker/ai-doc-picker-utils.test.js

portpro-frontends:
  M DocumentPicker/DocumentPickerTable.jsx     (+ DocPickerBucket + Customer col + select-all)
  M DocumentPicker/constants.js                (+ untrusted_url skip copy)
  M __tests__/.../DocumentPickerTable.test.jsx (+11 cases, 17 total)

portpro-ai-agents: clean
```

All uncommitted. Same hold-until-disputes-ship-env-fix-merges decision applies.

### Why no FE state-shape change

The new Customer column reads `doc.customerId` (BE already returned this in Session 3 — projected from `load.caller`) and `customer_labels` (BE already returned in Session 3 — populated via `CustomerModel.find` join in `listCustomerDocuments`). The select-all toggle operates on the existing `selectedIds: Set<string>` parent state. No new prop, no new BE field, no new SSE payload. Pure render-time addition.

