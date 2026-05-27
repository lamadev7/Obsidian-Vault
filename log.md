# Wiki Log

Append-only chronological record. Every ingest / query / lint pass gets one entry. Format: `## [YYYY-MM-DD] <op> | <title>`. Newest at the bottom (or top — pick one and stick with it; bottom is the unix-friendly default).

---

## [2026-05-27] bootstrap | wiki schema initialized

- Created [[CLAUDE.md]] defining LLM Wiki schema per Karpathy pattern.
- Scaffolded `raw/` for source intake.
- Initialized [[index.md]] cataloging existing pages: [[wiki/AI Agents/Google ADKs/Google ADKs]], [[wiki/AI Agents/Google ADKs/ADK Core Components]], [[wiki/AI Agents/Google ADKs/ADK Implementation Example]], [[wiki/Frontend/Frontend Folder Structure]].
- Created project skills: `/wiki-ingest`, `/wiki-query`, `/wiki-lint`.
- No source ingests yet.

## [2026-05-27] ingest | Karpathy, *LLM Wiki* — touched 6 pages: [[wiki/Skills/Karpathy LLM Wiki]], [[wiki/Skills/LLM Wiki Pattern]], [[wiki/Skills/Memex]], [[wiki/Skills/Compounding Knowledge vs RAG]], [[index]], [[log]]

- Source: [[raw/karpathy-llm-wiki]] (the schema's referent, now also formally ingested).
- Wrote source-summary [[wiki/Skills/Karpathy LLM Wiki]] capturing thesis, 3-layer architecture, operations, surprising claims, tooling, quotes.
- Created concept page [[wiki/Skills/LLM Wiki Pattern]] separating the abstract pattern from the source (so future sources can cite the pattern independently).
- Created entity page [[wiki/Skills/Memex]] for Vannevar Bush's 1945 precedent (referenced ≥2× in source, deserves its own page).
- Created comparison page [[wiki/Skills/Compounding Knowledge vs RAG]] — the core differentiator vs standard retrieval.
- All 4 pages frontmattered (`type` + `tags` + `sources` + `updated`) and bidirectionally interlinked.
- No contradictions with existing wiki content (legacy notes are isolated; no overlap with this source's domain).
- Updated [[index.md]] Skills section + Raw sources note.

## [2026-05-27] restructure | adopted strict Karpathy layout (wiki/ + raw/)

- Moved all topic dirs under new `wiki/` parent: `AI Agents/`, `Backend/`, `Frontend/`, `System Design/`, `Skills/`, `Daily/` → `wiki/<name>/`.
- Rewrote every internal wikilink across [[CLAUDE.md]], [[index.md]], [[log.md]], and all `wiki/Skills/*.md` pages from `[[Topic/Page]]` → `[[wiki/Topic/Page]]`.
- Updated [[CLAUDE.md]] § "Three layers" to reference `wiki/` topic dirs.
- Updated `/wiki-ingest` skill's topic-folder list to point under `wiki/`.
- `raw/` and schema layer (CLAUDE.md, index.md, log.md) unchanged at vault root.
- Result: clean 3-layer separation matching Karpathy's mental model — `raw/` (immutable), `wiki/` (LLM-owned), root files (schema + meta).

## [2026-05-27] realign | spec deviations corrected

- Moved `Skills/LLM WIKI Karpathy's.md` → [[raw/karpathy-llm-wiki]] (source doc belongs in `raw/`, not the wiki layer).
- Updated [[CLAUDE.md]] `@import` path to `@raw/karpathy-llm-wiki.md`.
- Stamped legacy pages with `type: legacy-note` frontmatter: [[wiki/Frontend/Frontend Folder Structure]], [[wiki/AI Agents/Google ADKs/Google ADKs]], [[wiki/AI Agents/Google ADKs/ADK Core Components]], [[wiki/AI Agents/Google ADKs/ADK Implementation Example]]. These are pre-schema hand-written notes; flagged for eventual re-ingest with proper citations + wikilinks.
- Updated [[index.md]]: tagged legacy pages, added [[raw/karpathy-llm-wiki]] under Raw sources, removed `Skills/LLM WIKI Karpathy's` reference (moved out).
- `Skills/` is now empty; `.gitkeep` added.
- Vault now structurally matches Karpathy's 3-layer spec.
