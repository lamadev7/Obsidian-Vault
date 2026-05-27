---
name: wiki-ingest
description: Ingest a raw source (article, paper, podcast transcript, web clip, PDF) into the LLM Wiki per the schema in CLAUDE.md. Read the source, summarize key takeaways, write a source-summary page in the best-fitting topic folder, update affected entity/concept pages with bidirectional wikilinks, update index.md, append a log.md entry. A single ingest typically touches 5-15 wiki pages. Triggers ingest, ingest this, add to wiki, file this source, process source, new article in raw/, integrate this paper, summarize and file.
---

# /wiki-ingest

Drive a full ingest pass per the schema in `CLAUDE.md`. Input: a path under `raw/` (or topic hint if multiple unprocessed). Output: updated wiki + index + log.

## Inputs

- `$ARGUMENTS` — relative path to source file under `raw/`, OR empty (then ask which unprocessed source to use, listing all `raw/` files not yet linked from `index.md`).

## Flow

1. **Read the source end-to-end.** Don't skim. If long, chunk-read fully.
2. **Summarize back to me** (3-7 bullets) covering: thesis, key entities, key concepts, surprising claims, contradictions with existing wiki content if any. **Wait for go-ahead** before writing pages unless the source is trivially small.
3. **Pick the topic folder.** Match to existing topic dirs under `wiki/`: `wiki/AI Agents/`, `wiki/Backend/`, `wiki/Frontend/`, `wiki/System Design/`, `wiki/Skills/`, `wiki/Daily/`. If none fit, propose a new topic dir under `wiki/` and ask.
4. **Write the source-summary page** in that folder. Filename mirrors the source slug in Title Case. Frontmatter `type: source-summary`, `sources: [[raw/<source>]]`, `updated: <today>`. Body: key claims, entities, concepts, quotes, links to existing wiki pages it relates to.
5. **Update entity / concept pages** the source touches.
   - For each entity (person, product, library, model, team) referenced ≥1 time: open or create their page. Add a section citing this source. Add bidirectional wikilinks.
   - For each concept (technique, pattern, principle): same treatment.
6. **Update `index.md`.** Add the new pages under the right category headings.
7. **Append to `log.md`** with this exact format:
   ```
   ## [YYYY-MM-DD] ingest | <source title> — touched N pages: [[a]], [[b]], [[c]], ...
   ```
8. **Report back** to me:
   - Pages written (with wikilink count)
   - Pages updated
   - Contradictions flagged (if any)
   - Suggested follow-ups: missing entity pages worth promoting, gaps worth a web search

## Hard rules

- **Never modify the raw source.** Read-only.
- **Every claim derived from the source must cite it** via wikilink: `[[raw/source.md|Title]]`.
- **Bidirectional links.** If page A references B, page B references A.
- **No orphans.** Every new page reachable from `index.md` (directly or transitively).
- **Frontmatter required** on every new/touched page.

## When to skip

- Source already linked from `index.md` (already ingested). Confirm with me before re-ingesting.
- Source not in `raw/`. Ask me to move it there first — sources outside `raw/` are not source-of-truth.
