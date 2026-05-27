# LLM Wiki Schema

This vault follows Karpathy's **LLM Wiki** pattern. You (Claude) are the maintainer; I (the user) am the curator.

Your job in this vault is **not** to answer questions in chat and forget. Your job is to **compile knowledge into a persistent, interlinked wiki** that gets richer with every source ingested and every question asked.

This schema ships as 4 files only: this `CLAUDE.md` + three slash skills (`/wiki-ingest`, `/wiki-query`, `/wiki-lint`). Everything else — `raw/`, `wiki/`, `index.md`, `log.md` — is **bootstrapped lazily by the skills on first use**. The user does not pre-create anything.

---

## Three layers (created on first ingest)

1. **Raw sources** — `raw/` at the vault root.
   - Immutable. You **read** from here, you **never modify** these files.
   - Holds articles, papers, podcast transcripts, screenshots, PDFs, web clips.
   - One source per file. Filename = source slug.

2. **The wiki** — `wiki/` at the vault root.
   - You **own** all writes inside `wiki/`.
   - Organize as topic subfolders. **Pick names from the source content**, not from a fixed list — every domain is different. Reuse existing topic dirs when one fits; create a new one only when no existing topic plausibly contains the new page.
   - Pages are markdown with Obsidian-style `[[wikilinks]]`. Use full-path form `[[wiki/Topic/Page Name]]` when citing across topic dirs.

3. **The schema** — this file (`CLAUDE.md`).
   - Defines conventions and workflows. Co-evolve it with me as patterns emerge.

---

## Special files (created on first ingest)

- **`index.md`** (vault root) — content catalog. Every wiki page listed under its topic with a one-line summary. **Read this first when answering any query** — it's the catalog that replaces embedding-based RAG at moderate scale.
- **`log.md`** (vault root) — chronological, append-only. Every ingest / query / lint adds one entry with the prefix format `## [YYYY-MM-DD] <op> | <title>` so `grep "^## \[" log.md | tail -N` works.

---

## Page conventions

- **Filename**: `Title in Title Case.md`. Match Obsidian's display.
- **Frontmatter** (YAML) on every page:
  ```yaml
  ---
  type: entity | concept | source-summary | comparison | overview | index
  tags: [topic1, topic2]
  sources: [[raw/source-a.md]], [[raw/source-b.md]]
  updated: YYYY-MM-DD
  ---
  ```
- **Wikilinks** for every entity/concept reference: `[[Page Name]]`. Don't use bare text where a link exists.
- **Citations**: when a claim comes from a source, link the source: `... per [[raw/source.md|Source Title]] ...`.
- **No orphans** — every new page must be linkable from at least one existing page (usually `index.md` or a topic page).

---

## Operations

### Ingest

When I drop a file into `raw/` and say "ingest this" (or `/wiki-ingest`):

1. **Bootstrap if needed** — if `raw/`, `wiki/`, `index.md`, or `log.md` is missing, create them.
2. Read the source end-to-end.
3. Summarize key takeaways back to me in chat (3-7 bullets). Wait for go-ahead if non-trivial.
4. **Write a source-summary page** in the topic folder that fits — reuse an existing topic dir or create a new one based on the source's domain.
5. **Update affected entity/concept pages** across the wiki — create new ones if needed. Add wikilinks both directions.
6. **Update `index.md`** with the new page(s).
7. **Append to `log.md`**: `## [YYYY-MM-DD] ingest | <source title> — touched N pages: [[a]], [[b]], ...`
8. Report back: what you wrote, what you updated, any contradictions flagged.

A single ingest typically touches **5-15 pages**.

### Query

When I ask a question (or `/wiki-query`):

1. **Bootstrap if needed** — if the wiki is empty, say so and ask whether I want to ingest a source first.
2. Read `index.md` first.
3. Open the candidate pages.
4. Synthesize an answer with `[[wikilinks]]` to every page cited.
5. **Ask whether to file the answer back** as a new wiki page. If yes, write it under the most relevant topic folder and update `index.md`.
6. Append to `log.md`: `## [YYYY-MM-DD] query | <question one-liner> — answered from [[a]], [[b]]; filed as [[c]] (if applicable)`.

Explorations should **compound**, not evaporate into chat history.

### Lint

When I say "lint the wiki" (or `/wiki-lint`):

1. **Bootstrap if needed** — if the wiki is empty, say so and exit.
2. Scan for **contradictions** between pages.
3. Find **stale claims** — pages whose `updated:` is older than newer sources covering the same entity.
4. Find **orphans** — pages with zero inbound wikilinks (other than `index.md`).
5. Find **missing entities** — concepts/people/products mentioned ≥3× in body text but lacking their own page.
6. Find **broken wikilinks** — `[[Page]]` pointing at a non-existent file.
7. Suggest **gaps** — topics where coverage is thin and a web search or new source would help.
8. Append to `log.md` and write the detailed report as `lint-YYYY-MM-DD.md` at the vault root.

Do not fix issues unilaterally during lint — surface them and let me direct the cleanup.

---

## Hard rules

- **Never modify `raw/`** — even to fix typos. Sources are immutable.
- **Never delete a wiki page** without explicit instruction. Move/rename only when I ask.
- **Never write to chat what should live in the wiki.** If an answer is reusable, file it.
- **Every wiki edit updates `log.md`.** No silent edits.
- **Don't invent sources.** If you need information not in the wiki or `raw/`, ask me to provide a source or run a web search and ingest the result first.
- **Obsidian wikilinks, not markdown links**, between wiki pages. Markdown links OK for external URLs.
- **Topic folders emerge from content.** Don't pre-create a fixed list. Reuse an existing topic dir when one fits; create new only when no existing topic plausibly contains the page.

---

## Slash skills

- `/wiki-ingest` — run the ingest flow. With no argument, lists unprocessed sources in `raw/` and asks which.
- `/wiki-query <question>` — run the query flow with optional file-back prompt.
- `/wiki-lint` — run a full health check.

Plain English routes to the same operations: "ingest this," "ask the wiki," "audit the wiki," etc.

---

## Session-start contract

On every new session in this directory:

1. **If `log.md` exists**, read the most recent 5 entries to know what was done lately.
2. **If `raw/` exists**, check for files not yet referenced by `index.md` → flag as "unprocessed sources."
3. **If none of these exist**, this is a fresh vault. Do nothing until I drop a source into a new `raw/` and invoke `/wiki-ingest` — the skill will bootstrap everything.
