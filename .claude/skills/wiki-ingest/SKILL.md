---
name: wiki-ingest
description: Ingest a raw source (article, paper, podcast transcript, web clip, PDF) into the LLM Wiki per the schema in CLAUDE.md. Bootstraps raw/, wiki/, index.md, and log.md on first use if missing. Reads the source, summarizes key takeaways, writes a source-summary page in a topic folder picked from the source's domain, updates affected entity/concept pages with bidirectional wikilinks, updates index.md, appends a log.md entry. A single ingest typically touches 5-15 wiki pages. Triggers ingest, ingest this, add to wiki, file this source, process source, new article in raw/, integrate this paper, summarize and file.
---

# /wiki-ingest

Drive a full ingest pass per the schema in `CLAUDE.md`. The skill is **self-bootstrapping** — it creates whatever structure is missing on first use.

## Step 0 — Bootstrap

Check the vault root. Create whatever is missing:

- If `raw/` does not exist → `mkdir raw && touch raw/.gitkeep`.
- If `wiki/` does not exist → `mkdir wiki`. **Do not pre-create topic subdirs** — they emerge from the source's content.
- If `index.md` does not exist → write a starter at the vault root:
  ```markdown
  ---
  type: index
  updated: <today>
  ---

  # Wiki Index

  Content catalog. One line per page. Updated on every ingest. Read first when answering a query.

  ## Raw sources (`raw/`)

  _(no sources ingested yet)_
  ```
- If `log.md` does not exist → write a starter at the vault root:
  ```markdown
  # Wiki Log

  Append-only chronological record. Format: `## [YYYY-MM-DD] <op> | <title>`. Newest at the bottom.

  ---

  ## [<today>] bootstrap | wiki initialized by /wiki-ingest
  ```

Report what you created (if anything) in one line: `Bootstrapped: raw/, wiki/, index.md, log.md` — then proceed.

## Step 1 — Pick the source

- If `$ARGUMENTS` is a path under `raw/` → use it.
- Else list every file in `raw/` not yet linked from `index.md` and ask me which one (or "all unprocessed").
- If `raw/` is empty → tell me to drop a source first and exit.

## Step 2 — Read

Read the source end-to-end. Don't skim. Chunk-read if long.

## Step 3 — Summarize

Summarize back to me (3-7 bullets) covering: thesis, key entities, key concepts, surprising claims, contradictions with existing wiki content if any. **Wait for go-ahead** before writing pages unless the source is trivially small.

## Step 4 — Pick the topic folder

Look at what `wiki/` already contains. Match the source to an existing topic subfolder if any fits.

If nothing fits, propose a new topic name based on the source's domain (e.g., `wiki/Machine Learning/`, `wiki/Drayage Operations/`, `wiki/Personal Health/`). Ask before creating.

**Do not pre-create a fixed list of topic folders.** Topics emerge from sources, one at a time.

## Step 5 — Write the source-summary page

In the chosen topic folder, write `Title in Title Case.md` with frontmatter:

```yaml
---
type: source-summary
tags: [<derived from content>]
sources: [[raw/<source-slug>]]
updated: <today>
---
```

Body: thesis, key claims, entities, concepts, quotes, and links to existing wiki pages it relates to.

## Step 6 — Cascade to entity/concept pages

For each entity (person, product, library, model, team) referenced ≥1× in the source: open or create their page in the most relevant topic folder. Add a section citing this source. Add bidirectional wikilinks.

Same treatment for concepts (techniques, patterns, principles).

## Step 7 — Update `index.md`

Add the new/updated pages under the right topic heading. Add the source under the "Raw sources" section with a note like "Ingested YYYY-MM-DD → see [[wiki/Topic/Source Summary Title]]".

## Step 8 — Append to `log.md`

Exact format:

```
## [YYYY-MM-DD] ingest | <source title> — touched N pages: [[a]], [[b]], [[c]], ...
```

## Step 9 — Report

Tell me:
- Pages written (with wikilink count per page)
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
- Source not in `raw/`. Ask me to move it there first.
