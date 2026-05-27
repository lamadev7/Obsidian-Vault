# LLM Wiki

An implementation of Karpathy's [*LLM Wiki*](raw/karpathy-llm-wiki.md) pattern: the LLM ingests raw sources once, compiles them into a persistent interlinked wiki, and keeps the wiki current as new sources arrive. You curate; the LLM does the bookkeeping.

You only ever need to say:

- **`/wiki-ingest`** — process a new source dropped into `raw/`
- **`/wiki-query <question>`** — answer from the wiki, optionally file the answer back
- **`/wiki-lint`** — health-check the wiki

Natural-language equivalents work too ("ingest this", "ask the wiki", "audit the wiki"). Claude follows the schema in `CLAUDE.md` with no further prompting.

---

## Three layers

| Layer | Location | Owner | Mutable? |
|---|---|---|---|
| Schema + meta | `/` (`CLAUDE.md`, `index.md`, `log.md`) | co-evolved | yes |
| Raw sources | `raw/` | you drop, no one edits | **immutable** |
| Wiki | `wiki/` | LLM | yes (LLM writes) |

---

## Quick start

1. Open the vault in Claude Code at the vault root.
2. Drop a source (article, paper, transcript, web clip) into `raw/`.
3. Type `/wiki-ingest`.
4. Claude reads the source, summarizes back to you, writes a source-summary page, updates affected entity/concept pages, updates `index.md`, appends `log.md`. One ingest typically touches 5–15 pages.
5. Ask questions with `/wiki-query` — Claude reads `index.md` first, synthesizes a cited answer, asks whether to file it back as a new wiki page.
6. Run `/wiki-lint` periodically to surface contradictions, orphans, broken links, missing entities, gaps.

---

## Setup from scratch

### 1. Scaffold

From the vault root:

```bash
mkdir -p raw wiki .claude/skills/wiki-ingest .claude/skills/wiki-query .claude/skills/wiki-lint
```

If you have existing topic notes, move them under `wiki/`.

### 2. Write the schema (`CLAUDE.md`)

`CLAUDE.md` at the vault root is what Claude auto-loads every session. It defines:

- The three layers and where they live
- Page conventions (frontmatter, wikilinks, citations)
- The three operations (Ingest, Query, Lint)
- Hard rules (raw is immutable, no orphan pages, every edit logs)

Copy `CLAUDE.md` from this repo as a template and adapt it to your domain.

### 3. Create `index.md` and `log.md`

`index.md` — content catalog, one line per wiki page, organized by topic. Updated on every ingest.

`log.md` — append-only chronological log. Every operation adds one entry with a parseable prefix:

```
## [YYYY-MM-DD] ingest | <title> — touched N pages: [[a]], [[b]], ...
## [YYYY-MM-DD] query  | <question> — answered from [[a]], [[b]]; filed as [[c]]
## [YYYY-MM-DD] lint   | N issues — see [[lint-YYYY-MM-DD]]
```

### 4. Add the three slash skills

Skills live under `.claude/skills/<name>/SKILL.md`. Each file starts with YAML frontmatter:

```yaml
---
name: wiki-ingest
description: <one-paragraph description with trigger keywords so plain English routes here too>
---
```

…followed by the operation flow. Copy the three `SKILL.md` files from `.claude/skills/` in this repo as templates.

The `description` field matters — Claude uses it to match trigger phrases like "ingest this" or "audit the wiki," so users don't need to remember the slash command.

### 5. First ingest

Drop the Karpathy source into `raw/`, then run `/wiki-ingest`. Claude will build the first source-summary, entity, and concept pages — that's the loop working end-to-end.

---

## Tips

- **One source at a time, stay involved.** Karpathy's recommendation. The summary discussion catches misclassifications early.
- **File queries back aggressively.** Any synthesis only living in chat is wasted. The wiki compounds when explorations stick.
- **Lint monthly.** Catch orphans and stale claims before they decay further.
- **Don't write wiki pages by hand.** Let the LLM do it. If you must edit, do it through Claude in dialogue so it logs and propagates correctly.
- **Use Obsidian's graph view** to see which pages are hubs and which are orphans.
- **Treat `CLAUDE.md` as a living document.** When a pattern needs enforcing consistently, add it to the schema instead of repeating yourself per session.

---

## Credits

- Pattern: [Andrej Karpathy, *LLM Wiki*](raw/karpathy-llm-wiki.md)
- Precedent: Vannevar Bush, *As We May Think* (1945) — the Memex
- IDE: [Obsidian](https://obsidian.md)
- Programmer: [Claude Code](https://claude.com/claude-code)
