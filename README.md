# LLM Wiki

An implementation of Karpathy's [*LLM Wiki*](raw/karpathy-llm-wiki.md) pattern: the LLM ingests raw sources once, compiles them into a persistent interlinked wiki, and keeps the wiki current as new sources arrive. You curate; the LLM does the bookkeeping.

You only ever need to say:

- **`/wiki-ingest`** — process a new source
- **`/wiki-query <question>`** — answer from the wiki, optionally file the answer back
- **`/wiki-lint`** — health-check the wiki

Natural-language equivalents work too ("ingest this", "ask the wiki", "audit the wiki"). Claude follows the schema in `CLAUDE.md` with no further prompting.

---

## Three layers (auto-created on first ingest)

| Layer | Location | Owner | Mutable? |
|---|---|---|---|
| Schema | `CLAUDE.md` | co-evolved | yes |
| Raw sources | `raw/` | you drop, no one edits | **immutable** |
| Wiki | `wiki/` | LLM | yes (LLM writes) |
| Catalog | `index.md` | LLM | yes |
| Log | `log.md` | LLM (append-only) | yes |

You pre-create **nothing**. The first `/wiki-ingest` bootstraps `raw/`, `wiki/`, `index.md`, and `log.md` if they don't exist. Topic subfolders inside `wiki/` emerge from your source content — no fixed list.

---

## Setup

Run these steps from your vault root.

### Step 1 — Create the three layer folders

```bash
mkdir -p raw wiki output
```

### Step 2 — Move existing notes into `wiki/`

Skip if your vault is empty. Otherwise, in Obsidian (or Finder/Explorer), **drag and drop all your existing files and folders into the `wiki/` folder** so the LLM-owned layer is properly nested.

### Step 3 — Pull `CLAUDE.md` + the three skill files

```bash
mkdir -p .claude/skills/wiki-ingest .claude/skills/wiki-query .claude/skills/wiki-lint
BASE=https://raw.githubusercontent.com/lamadev7/my-second-brain/main
curl -fsSL $BASE/CLAUDE.md                            -o CLAUDE.md
curl -fsSL $BASE/.claude/skills/wiki-ingest/SKILL.md  -o .claude/skills/wiki-ingest/SKILL.md
curl -fsSL $BASE/.claude/skills/wiki-query/SKILL.md   -o .claude/skills/wiki-query/SKILL.md
curl -fsSL $BASE/.claude/skills/wiki-lint/SKILL.md    -o .claude/skills/wiki-lint/SKILL.md
```

Or grab the files manually from GitHub:

- [CLAUDE.md](https://github.com/lamadev7/my-second-brain/blob/main/CLAUDE.md)
- [.claude/skills/wiki-ingest/SKILL.md](https://github.com/lamadev7/my-second-brain/blob/main/.claude/skills/wiki-ingest/SKILL.md)
- [.claude/skills/wiki-query/SKILL.md](https://github.com/lamadev7/my-second-brain/blob/main/.claude/skills/wiki-query/SKILL.md)
- [.claude/skills/wiki-lint/SKILL.md](https://github.com/lamadev7/my-second-brain/blob/main/.claude/skills/wiki-lint/SKILL.md)

### Step 4 — Restart Claude Code

Exit (`Ctrl-D` or `/exit`) and re-open Claude in this directory so it picks up `CLAUDE.md` and registers the three new skills. The first `/wiki-ingest` is now one drop-and-type away.

---

## Use

1. Drop a source (article, paper, transcript, web clip) into the vault root. The first ingest will move it into `raw/`.
2. Type `/wiki-ingest`. Claude:
   - Bootstraps `raw/`, `wiki/`, `index.md`, `log.md` if first run
   - Reads the source, summarizes back to you (3–7 bullets)
   - Writes a source-summary page in a topic folder picked from the content
   - Cascades to entity/concept pages with bidirectional wikilinks
   - Updates `index.md`, appends `log.md`
3. Ask questions with `/wiki-query` — Claude reads `index.md` first, synthesizes a cited answer, asks whether to file it back as a new wiki page.
4. Run `/wiki-lint` periodically — surfaces contradictions, orphans, broken links, missing entities, gaps. Doesn't fix without your direction.

---

## Tips

- **One source at a time, stay involved.** The summary discussion catches misclassifications early.
- **File queries back aggressively.** Synthesis that lives only in chat is wasted.
- **Lint monthly.** Catch orphans and stale claims before they decay further.
- **Don't write wiki pages by hand.** Let the LLM do it so it logs and propagates correctly.
- **Use Obsidian's graph view** to see which pages are hubs and which are orphans.
- **Treat `CLAUDE.md` as living.** When a pattern needs enforcing consistently, add it to the schema instead of repeating yourself.

---

## Credits

- Pattern: [Andrej Karpathy, *LLM Wiki*](raw/karpathy-llm-wiki.md)
- Precedent: Vannevar Bush, *As We May Think* (1945) — the Memex
- IDE: [Obsidian](https://obsidian.md)
- Programmer: [Claude Code](https://claude.com/claude-code)
