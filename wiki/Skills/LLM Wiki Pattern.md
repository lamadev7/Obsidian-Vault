---
type: concept
tags: [knowledge-management, llm-wiki, methodology, pattern]
sources: [[raw/karpathy-llm-wiki]]
updated: 2026-05-27
---

# LLM Wiki Pattern

A pattern for personal knowledge bases where an LLM agent **incrementally builds and maintains** a persistent, interlinked markdown wiki sitting between the user and raw source documents. Defined by [[raw/karpathy-llm-wiki|Karpathy (2026)]]; full source-summary at [[Karpathy LLM Wiki]].

## Definition

Knowledge is **compiled once and kept current**, not re-derived on every query. The wiki is a compounding artifact — every source ingested and every question asked makes it richer.

Contrast: [[Compounding Knowledge vs RAG]].

## Structure

Three layers, strict separation:

1. **Raw sources** (immutable) — articles, papers, transcripts, clips. LLM reads, never modifies.
2. **Wiki** (LLM-owned) — summaries, entity pages, concept pages, comparisons. Markdown with interlinks. User reads, LLM writes.
3. **Schema** (co-evolved) — a configuration doc (CLAUDE.md, AGENTS.md) telling the LLM the conventions + workflows for *this* user's domain.

## Roles

| Actor | Job |
|---|---|
| Human | Curate sources, ask good questions, direct the analysis, evolve the schema |
| LLM | Read, summarize, file, cross-reference, maintain consistency, lint |
| Wiki | Single artifact compounding from both |

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase." — [[raw/karpathy-llm-wiki|Karpathy]]

## Core operations

- **Ingest** — pull a new source into the wiki, touching 10–15 pages on average.
- **Query** — answer questions against the wiki, optionally *file the answer back* as a new page so explorations compound.
- **Lint** — periodic health check (contradictions, stale claims, orphans, missing entities, broken links).

## Why it works

The hard part of a knowledge base isn't reading or thinking — it's **bookkeeping**: updating cross-references, keeping summaries current, flagging when new data contradicts old claims. Humans abandon wikis because maintenance grows faster than value. LLMs don't get bored, don't forget cross-references, can touch 15 files in one pass. Maintenance cost approaches zero.

## Scale notes

- Index file alone scales to ~100 sources / hundreds of pages.
- Beyond that, add proper search ([[Karpathy LLM Wiki|qmd or similar]]).
- The pattern is modular — pick what fits your domain, ignore what doesn't.

## Implementations

- **This vault** — [[CLAUDE.md]] is the schema; `raw/` is the source layer; topic dirs are the wiki; `/wiki-ingest` `/wiki-query` `/wiki-lint` are the operations.
- **Karpathy's original suggestion** — works equally well with OpenAI Codex, Claude Code, OpenCode/Pi, or any LLM agent that can read+write files.

## Historical precedent

[[Memex]] (Vannevar Bush, 1945) is the spiritual ancestor: personal, curated, associative-trail-based. The maintenance burden was the unsolved piece — LLMs close that gap.

## Related

- [[Karpathy LLM Wiki]] — primary source-summary
- [[Memex]] — historical precedent
- [[Compounding Knowledge vs RAG]] — what makes this different from standard retrieval
- [[CLAUDE.md]] — concrete schema for this vault
