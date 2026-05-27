---
name: wiki-query
description: Answer a question against the LLM Wiki by reading index.md first, drilling into candidate pages, and synthesizing a cited answer with wikilinks. Optionally file the answer back as a new wiki page so explorations compound instead of evaporating into chat. Triggers ask the wiki, what does the wiki say, query the wiki, search my notes, look up in the wiki, synthesize from notes, what do I have on X.
---

# /wiki-query

Run the query flow per `CLAUDE.md`. Input: a question. Output: synthesized answer with citations + optional file-back as a new wiki page.

## Inputs

- `$ARGUMENTS` — the question. If empty, ask me what to query.

## Flow

1. **Read `index.md` first.** Always. It's the catalog.
2. **Identify candidate pages.** From `index.md` summaries, pick pages likely to contain the answer. Open them.
3. **If the wiki is thin on this topic**, say so explicitly and ask whether to run a web search + ingest first (don't just hallucinate).
4. **Synthesize the answer.** Every claim must be backed by a `[[wikilink]]` to the source wiki page (and through it, to the raw source). No uncited claims.
5. **Format the answer to fit the question:**
   - Conceptual / comparison → markdown with a comparison table
   - "How do I do X" → numbered steps
   - "What does X think about Y" → quoted excerpts with citations
   - Quantitative → chart if useful (note: ask me before writing chart code)
6. **Ask: "File this back as a wiki page?"** If yes:
   - Pick the right topic folder.
   - Write the page with frontmatter `type: comparison | analysis | overview | faq`, `sources: [[a]], [[b]]`, `updated: <today>`.
   - Wikilink it from `index.md` and from the parent topic page if one exists.
7. **Append to `log.md`:**
   ```
   ## [YYYY-MM-DD] query | <question one-liner> — answered from [[a]], [[b]]; filed as [[c]] (if applicable)
   ```

## Hard rules

- **Index-first.** Never grep raw/ blind — read `index.md` first to know what wiki pages exist.
- **Cite or refuse.** If you can't find a wiki page backing a claim, either say "the wiki does not cover this" or run an explicit web search + ingest cycle.
- **File-back is opt-in but encouraged.** Default to asking, not assuming.
- **No silent edits.** Even if the answer is just a quick chat response, log it if it referenced wiki pages.
