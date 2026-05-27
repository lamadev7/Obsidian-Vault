---
name: wiki-query
description: Answer a question against the LLM Wiki per the schema in CLAUDE.md. Reads index.md first, drills into candidate pages, synthesizes a cited answer with wikilinks. Optionally files the answer back as a new wiki page so explorations compound. If the wiki is empty / not yet bootstrapped, tells the user to ingest a source first. Triggers ask the wiki, what does the wiki say, query the wiki, search my notes, look up in the wiki, synthesize from notes, what do I have on X.
---

# /wiki-query

Run the query flow per `CLAUDE.md`.

## Step 0 — Bootstrap check

- If `index.md` does not exist OR `wiki/` is empty → tell me: "The wiki is empty. Drop a source into `raw/` and run `/wiki-ingest` first." Exit.
- If `log.md` does not exist but `index.md` does → create `log.md` with the starter header before proceeding (so the query gets logged).

## Step 1 — Question

`$ARGUMENTS` = the question. If empty, ask me what to query.

## Step 2 — Read the catalog first

Read `index.md`. Always. It's the catalog and it's where you find what wiki pages exist.

## Step 3 — Identify candidate pages

From `index.md` summaries, pick pages likely to contain the answer. Open them.

If coverage looks thin → say so explicitly and ask whether to run a web search + ingest before answering. Don't hallucinate.

## Step 4 — Synthesize

Every claim must be backed by a `[[wikilink]]` to the wiki page (and through it, to the raw source). No uncited claims.

Format to fit the question:
- Conceptual / comparison → markdown with a comparison table
- "How do I do X" → numbered steps
- "What does X think about Y" → quoted excerpts with citations
- Quantitative → chart if useful (ask before writing chart code)

## Step 5 — File-back prompt

Ask: **"File this back as a wiki page?"**

If yes:
- Pick the right topic folder.
- Write the page with frontmatter (`type: comparison | analysis | overview | faq`, `sources:`, `updated:`).
- Wikilink it from `index.md` and from the parent topic page if one exists.

## Step 6 — Log

Append to `log.md`:

```
## [YYYY-MM-DD] query | <question one-liner> — answered from [[a]], [[b]]; filed as [[c]] (if applicable)
```

## Hard rules

- **Index-first.** Never grep `wiki/` blind. Read `index.md` first to know what pages exist.
- **Cite or refuse.** If you can't find a wiki page backing a claim, either say "the wiki does not cover this" or run an explicit web search + ingest cycle.
- **File-back is opt-in but encouraged.** Default to asking, not assuming.
- **Always log.** Even quick chat answers that referenced wiki pages get a log entry.
