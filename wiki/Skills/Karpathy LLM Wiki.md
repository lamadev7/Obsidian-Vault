---
type: source-summary
tags: [llm, knowledge-management, llm-wiki, methodology]
sources: [[raw/karpathy-llm-wiki]]
updated: 2026-05-27
---

# Karpathy — LLM Wiki

Source-summary of [[raw/karpathy-llm-wiki|Karpathy, *LLM Wiki*]]. An intentionally abstract pattern description for using an LLM agent to incrementally build and maintain a personal knowledge wiki between you and raw sources.

## Thesis

Standard RAG (NotebookLM, ChatGPT file uploads) re-derives knowledge on every query — nothing accumulates. The [[LLM Wiki Pattern]] inverts this: the LLM *compiles* incoming sources into a persistent wiki and *keeps it current*. See [[Compounding Knowledge vs RAG]] for the contrast.

> "The wiki is a persistent, compounding artifact." — [[raw/karpathy-llm-wiki|Karpathy]]

## Architecture (3 layers)

1. **Raw sources** — immutable. LLM reads, never modifies.
2. **The wiki** — LLM-owned markdown. Summaries, entity pages, concept pages, comparisons, overview, synthesis. *"You read it; the LLM writes it."*
3. **The schema** — config doc (CLAUDE.md / AGENTS.md) telling the LLM the conventions and workflows. Co-evolved with the user.

Implemented in this vault via [[CLAUDE.md]] (schema) + topic dirs (wiki) + `raw/` (sources).

## Operations

| Op | Trigger | Behavior |
|---|---|---|
| **Ingest** | Drop new source, ask LLM to process | LLM reads, discusses takeaways, writes summary page, updates entity/concept pages across the wiki, updates index, appends log. One ingest touches 10–15 pages. |
| **Query** | Ask a question | LLM searches index, reads pages, synthesizes cited answer. Good answers get *filed back* as new wiki pages — explorations compound. |
| **Lint** | Periodic health check | Find contradictions, stale claims, orphans, missing entities, broken links, gaps worth a web search. |

Wired in this vault as `/wiki-ingest`, `/wiki-query`, `/wiki-lint` slash skills.

## Special files

- **`index.md`** — content catalog. Per-page line with one-line summary, organized by category. Read first when answering a query. *"Avoids the need for embedding-based RAG infrastructure at moderate scale (~100 sources)."*
- **`log.md`** — chronological, append-only. Format `## [YYYY-MM-DD] op | title` so `grep "^## \[" log.md | tail -5` works.

## Surprising claims

- **Index file alone scales to ~100 sources / hundreds of pages** — no embeddings / vector DB needed at moderate scale.
- **Maintenance, not creation, is the bottleneck** — humans abandon wikis because maintaining cross-references is tedious; LLMs don't get bored.
- **The schema document IS the configuration** — what makes an LLM "a disciplined wiki maintainer rather than a generic chatbot."
- **Spiritually descended from Vannevar Bush's [[Memex]] (1945)** — Bush's vision was private + curated + associative-trails, closer to this than what the web became. The piece Bush couldn't solve was *who does the maintenance*. LLMs.

## Tools mentioned

- **Obsidian** — the IDE for the human side. Graph view shows wiki shape.
- **Obsidian Web Clipper** — browser ext, web → markdown into `raw/`.
- **qmd** — local markdown search (BM25 + vector + LLM rerank); CLI + MCP server. By Tobi. Optional, useful as wiki grows past index-only scale.
- **Marp** — markdown slide decks. Generate presentations from wiki content.
- **Dataview** — Obsidian plugin, queries page frontmatter (justifies the `type:` / `updated:` / `tags:` we enforce).

## Domains it applies to

Personal (self-tracking, journals), research (deep topic dives), reading a book (companion wiki à la Tolkien Gateway), business/team (internal wiki fed by Slack/meetings), competitive analysis, due diligence, trip planning, course notes, hobby deep-dives. Anywhere knowledge accumulates over time.

## Why this matters here

This source IS the schema's referent — [[CLAUDE.md]] imports it and defines the concrete instantiation for this vault. Every operational rule in this vault traces back to this document.

## Related

- [[LLM Wiki Pattern]] — the abstract pattern (separate from this source)
- [[Memex]] — intellectual ancestor
- [[Compounding Knowledge vs RAG]] — the core differentiator
- [[CLAUDE.md]] — this vault's schema, derived from the source
