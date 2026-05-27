# Tech Vault — LLM Wiki Schema

This vault follows Karpathy's **LLM Wiki** pattern. You (Claude) are the maintainer; I (the user) am the curator. The full architectural rationale is in @raw/karpathy-llm-wiki.md — read it once per session if you have not already.

Your job in this vault is **not** to answer questions in chat and forget. Your job is to **compile knowledge into a persistent, interlinked wiki** that gets richer with every source ingested and every question asked.

---

## Three layers

1. **Raw sources** — `raw/` at the vault root.
   - Immutable. You **read** from here, you **never modify** these files.
   - Contains articles, papers, podcast transcripts, screenshots, PDFs, web clips.
   - One source per file (or per folder if a source has attachments). Filename = source slug.

2. **The wiki** — everything under `wiki/`:
   - Topic dirs: `wiki/AI Agents/`, `wiki/Backend/`, `wiki/Frontend/`, `wiki/System Design/`, `wiki/Skills/`, `wiki/Daily/`.
   - You **own** all writes inside `wiki/`.
   - Each folder is a topic cluster. Add subfolders freely as topics deepen.
   - Pages are markdown notes with Obsidian-style `[[wikilinks]]`. Use full-path form `[[wiki/Topic/Page Name]]` when citing across topic dirs to keep links explicit and grep-friendly.

3. **The schema** — this file (`CLAUDE.md`).
   - Defines conventions and workflows below. Co-evolve it with me as patterns emerge.

---

## Special files

- **`index.md`** (vault root) — content catalog. Every wiki page listed under a category with a one-line summary. Update on every ingest. Read first when answering a query to locate relevant pages before drilling in.
- **`log.md`** (vault root) — chronological, append-only. Every ingest / query / lint pass gets an entry. Use the prefix format `## [YYYY-MM-DD] <op> | <title>` so `grep "^## \[" log.md | tail -N` works.

---

## Page conventions

- **Filename**: `Title in Title Case.md`. No dashes-as-spaces. Match Obsidian's display.
- **Frontmatter** (YAML) on every page:
  ```yaml
  ---
  type: entity | concept | source-summary | comparison | overview | index
  tags: [topic1, topic2]
  sources: [[raw/source-a.md]], [[raw/source-b.md]]
  updated: 2026-05-27
  ---
  ```
- **Wikilinks** for every entity/concept reference: `[[Page Name]]`. Don't use bare text where a link exists.
- **Citations**: when a claim comes from a source, link the source: `... per [[raw/karpathy-llm-wiki.md|Karpathy]] ...`.
- **No orphans** — every new page must be linkable from at least one existing page (usually the relevant topic index or `index.md`).

---

## Operations

### Ingest

When I drop a file into `raw/` and say "ingest this":

1. Read the source end-to-end.
2. Summarize key takeaways back to me in chat (3-7 bullets). Wait for go-ahead if non-trivial.
3. **Write a source-summary page** in the topic folder it best fits (or create a new topic subfolder). Filename mirrors the source.
4. **Update affected entity/concept pages** across the wiki — create new ones if needed. Add wikilinks both directions.
5. **Update `index.md`** with the new page(s).
6. **Append to `log.md`**: `## [YYYY-MM-DD] ingest | <source title> — touched N pages: [[a]], [[b]], ...`
7. Report back: what you wrote, what you updated, any contradictions you flagged.

A single ingest typically touches **5-15 pages**. Don't be shy about cross-referencing.

### Query

When I ask a question:

1. Read `index.md` first.
2. Open the candidate pages.
3. Synthesize an answer with `[[wikilinks]]` to every page cited.
4. **Ask whether to file the answer back** as a new wiki page (comparison, analysis, FAQ entry, decision record). If yes, write it under the most relevant topic folder and update `index.md`.
5. Append to `log.md`: `## [YYYY-MM-DD] query | <question one-liner> — answered from [[a]], [[b]]; filed as [[c]] (if applicable)`.

Explorations should **compound**, not evaporate into chat history.

### Lint

When I say "lint the wiki":

1. Scan for **contradictions** between pages — flag with a one-line summary + which pages disagree.
2. Find **stale claims** — pages whose `updated:` is old AND whose sources have been superseded by newer ingests.
3. Find **orphans** — pages with zero inbound wikilinks.
4. Find **missing entities** — concepts/people/products mentioned ≥3 times in body text but lacking their own page.
5. Find **broken wikilinks** — `[[Page]]` pointing at a non-existent file.
6. Suggest **gaps** — topics where existing sources are thin and a web search or new source would help.
7. Append to `log.md`: `## [YYYY-MM-DD] lint | N issues — see [[lint-2026-05-27.md]]` and write the detailed report as that page.

Do not fix issues unilaterally during lint — surface them and let me direct the cleanup.

---

## Hard rules

- **Never modify `raw/`** — even to fix typos. Sources are immutable.
- **Never delete a wiki page** without explicit instruction. Move/rename only when I ask.
- **Never write to chat what should live in the wiki.** If an answer is reusable, file it.
- **Every wiki edit updates `log.md`.** No silent edits.
- **Don't invent sources.** If you need information not in the wiki or `raw/`, ask me to provide a source or run a web search and ingest the result first.
- **Obsidian wikilinks, not markdown links**, between wiki pages. Markdown links OK for external URLs.

---

## Slash skills

Three project skills wrap the operations above for explicit invocation:

- `/wiki-ingest <path|topic>` — run the ingest flow for a specific source or topic.
- `/wiki-query <question>` — run the query flow with optional file-back prompt.
- `/wiki-lint` — run a full health check.

These call the same workflows defined here. Without them, plain English ("ingest the new article in raw/", "lint", "what does X say about Y") routes to the same operations.

---

## Session-start contract

On every new session in this directory:

1. Confirm `index.md` and `log.md` are current with the actual wiki state (spot-check 3 random pages).
2. Read the most recent 5 entries of `log.md` to know what was done lately.
3. If `raw/` has files not yet referenced by any wiki page → flag them as "unprocessed sources" to me.
