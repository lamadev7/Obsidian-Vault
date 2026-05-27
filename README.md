# LLM Wiki — an Obsidian vault Claude maintains for you

An implementation of [Karpathy's *LLM Wiki* pattern](raw/karpathy-llm-wiki.md): the LLM ingests raw sources once, *compiles* them into a persistent interlinked wiki, and keeps the wiki current as new sources arrive. You curate; the LLM does the bookkeeping.

The whole vault is wired so that you only ever say things like:

- **`/wiki-ingest`** — "process the source I just dropped into `raw/`"
- **`/wiki-query <question>`** — "answer this from the wiki, file the answer back if useful"
- **`/wiki-lint`** — "health-check the wiki"

…and Claude follows the schema in [`CLAUDE.md`](CLAUDE.md) without any further prompting. On session exit, every change auto-pushes to GitHub on the `claude/autosave` branch, with a multi-layer secret scan blocking anything that looks like a credential.

---

## Architecture

Three layers, strict separation:

```
Tech/                       # vault root
├── CLAUDE.md               # the SCHEMA — Claude auto-loads this every session
├── README.md               # this file
├── index.md                # content catalog (read first on every query)
├── log.md                  # append-only event log
│
├── raw/                    # LAYER 1: immutable sources
│   └── karpathy-llm-wiki.md
│
└── wiki/                   # LAYER 2: LLM-owned synthesis
    ├── AI Agents/
    ├── Backend/
    ├── Frontend/
    ├── System Design/
    ├── Skills/             # methodology / patterns / entity pages
    │   ├── Karpathy LLM Wiki.md
    │   ├── LLM Wiki Pattern.md
    │   ├── Memex.md
    │   └── Compounding Knowledge vs RAG.md
    └── Daily/
```

| Layer | Owner | Mutable? | Purpose |
|---|---|---|---|
| Schema + meta (`/`) | Co-evolved with you | Yes | How the LLM operates |
| `raw/` | You drop, no one edits | **No — immutable** | Source of truth |
| `wiki/` | LLM | Yes (LLM writes) | Compiled, interlinked knowledge |

---

## Quick start (using this vault)

1. Open this directory in Claude Code (`claude` in the terminal at the vault root, or open the Claude Code IDE extension here).
2. Claude auto-loads `CLAUDE.md` → it now knows the schema.
3. Drop a source into `raw/`. Any markdown, plain text, or article you've clipped.
4. Type one of:

```
/wiki-ingest
```

Or natural-language equivalents — Claude routes them to the same flow:

```
ingest the new article in raw/
file this source
process my-new-source.md
```

5. Claude reads the source, summarizes back to you, writes a source-summary page, updates affected entity/concept pages, updates `index.md`, appends `log.md`. One ingest typically touches 5–15 pages.

Same idea for queries:

```
/wiki-query what's the difference between RAG and the LLM Wiki pattern?
```

…or just ask in plain English: "from the wiki, what's the difference between RAG and the LLM Wiki pattern?" — Claude reads `index.md` first, drills into pages, synthesizes with `[[wikilinks]]`, and asks whether to file the answer back as a new wiki page.

And for housekeeping:

```
/wiki-lint
```

…or "audit the wiki" — finds contradictions, orphans, broken links, missing entities, topic gaps. Surfaces them; doesn't fix without your direction.

---

## Setup from scratch (replicating this on another vault)

### Prerequisites

- An Obsidian vault (or any directory of markdown files)
- A git remote you control (`git init` + `git remote add origin <url>` if starting fresh)
- Claude Code installed: `npm install -g @anthropic-ai/claude-code` (or use the IDE extension)

### Step 1 — scaffold the layout

From the vault root:

```bash
mkdir -p raw wiki .claude/skills/wiki-ingest .claude/skills/wiki-query .claude/skills/wiki-lint
touch raw/.gitkeep
```

If you already have topic notes, move them under `wiki/`:

```bash
# example — adjust to your topics
for d in "AI Agents" Backend Frontend "System Design" Skills Daily; do
  [ -d "$d" ] && mv "$d" "wiki/$d"
done
```

### Step 2 — write the schema (`CLAUDE.md`)

Create `CLAUDE.md` at the vault root. This is the file Claude auto-loads every session. It tells Claude:

- The three layers and where they live
- Page conventions (frontmatter, wikilinks, citations)
- The three operations (Ingest, Query, Lint)
- Hard rules (raw is immutable, no orphan pages, every edit logs to `log.md`)
- The session-start contract (read recent log, flag unprocessed sources)

Copy `CLAUDE.md` from this repo as a starting point and adapt it to your domain. Karpathy himself says: *"the schema is what makes the LLM a disciplined wiki maintainer rather than a generic chatbot."*

### Step 3 — create `index.md` and `log.md`

Both live at the vault root.

**`index.md`** — content catalog, one line per wiki page, organized by topic. Updated on every ingest. Start with a section per topic dir and "(no pages yet)" placeholders.

**`log.md`** — append-only chronological log. Every ingest / query / lint adds one entry with the prefix format:

```
## [YYYY-MM-DD] ingest | <title> — touched N pages: [[a]], [[b]], ...
## [YYYY-MM-DD] query  | <question> — answered from [[a]], [[b]]; filed as [[c]]
## [YYYY-MM-DD] lint   | N issues — see [[lint-YYYY-MM-DD]]
```

The prefix makes the log parseable with simple unix tools: `grep "^## \[" log.md | tail -5`.

### Step 4 — add the three slash skills

Skills live under `.claude/skills/<name>/SKILL.md`. Each is a self-contained instruction file Claude loads when you invoke `/<name>` or use trigger phrases.

Create three:

```
.claude/skills/wiki-ingest/SKILL.md
.claude/skills/wiki-query/SKILL.md
.claude/skills/wiki-lint/SKILL.md
```

Each `SKILL.md` starts with YAML frontmatter:

```yaml
---
name: wiki-ingest
description: <one-paragraph description with trigger keywords so plain-English routes here too>
---
```

…followed by the operation flow (read source → summarize → write pages → update index → append log). Copy the three skill files from `.claude/skills/` in this repo as templates.

The descriptions matter: Claude uses them to match trigger phrases. Include words like "ingest this," "file this source," "lint the wiki," etc., so users don't need to remember the slash command.

### Step 5 — drop the Karpathy doc into `raw/`

```bash
# get the source file (your own copy)
curl -L <url-to-karpathy-llm-wiki.md> -o raw/karpathy-llm-wiki.md
```

Then run `/wiki-ingest` in Claude Code. Claude will read it, build a source-summary page in `wiki/Skills/`, create concept pages (`LLM Wiki Pattern`, `Memex`, `Compounding Knowledge vs RAG`), and demonstrate the loop.

### Step 6 — `.gitignore`

Recommended `.gitignore` at the vault root:

```gitignore
# secrets
.env
.env.*
*.pem
*.key
*credentials*
*secret*
.npmrc
.aws/
.ssh/

# OS / editor cruft
.DS_Store
Thumbs.db
*.swp

# obsidian local state (paths leak personal info)
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/.DS_Store

# build artifacts
node_modules/
dist/
.venv/
__pycache__/
```

### Step 7 — (optional) auto-push hook

If you want every Claude session's work auto-pushed to GitHub on exit, install a `SessionEnd` hook.

**Script** — `~/.claude/hooks/git-autosave.sh`:

```bash
#!/usr/bin/env bash
# Auto-commit + push to claude/autosave branch on session end.
# Aborts if any file or staged diff line matches a secret pattern.
# Source: see git-autosave.sh in this repo's documentation.
```

The full version (with 20+ filename patterns and 21 content regexes for AWS / GitHub / Anthropic / OpenAI / Slack / Atlassian / NewRelic / GitLab / Google / Mongo / Postgres / JWT / PEM keys / generic `FOO_KEY="..."`) is in this repo at — see commit history or copy from `~/.claude/hooks/git-autosave.sh` after installation.

**Wire it in** — add to `~/.claude/settings.json`:

```json
{
  "hooks": {
    "SessionEnd": [
      {
        "hooks": [
          { "type": "command", "command": "bash /Users/<you>/.claude/hooks/git-autosave.sh" }
        ]
      }
    ]
  }
}
```

Make the script executable: `chmod +x ~/.claude/hooks/git-autosave.sh`.

On every session exit the hook will:

1. Skip if not a git repo, no `origin` remote, or no changes
2. Refuse to stage any file matching the filename blocklist (`.env`, `*.pem`, `*credentials*`, etc.)
3. Stage all changes, scan the staged diff against the content regexes — **abort and reset** if any pattern matches
4. If clean: stash WIP, switch to `claude/autosave` branch (create if missing), apply stash, commit with a meaningful message (`chore(autosave): session save — N change(s): <top files>`), push, return to original branch, restore WIP

Logs to `~/.claude/hooks/git-autosave.log` — inspect after any session to see what was committed or why an abort fired.

---

## File reference

| File | Purpose |
|---|---|
| [`CLAUDE.md`](CLAUDE.md) | Schema — auto-loaded by Claude every session in this dir |
| [`index.md`](index.md) | Content catalog, organized by topic |
| [`log.md`](log.md) | Chronological event log (ingest / query / lint / structural changes) |
| [`raw/`](raw/) | Source documents, immutable |
| [`wiki/`](wiki/) | LLM-owned synthesis (topic dirs) |
| [`.claude/skills/wiki-ingest/`](.claude/skills/wiki-ingest/) | `/wiki-ingest` flow definition |
| [`.claude/skills/wiki-query/`](.claude/skills/wiki-query/) | `/wiki-query` flow definition |
| [`.claude/skills/wiki-lint/`](.claude/skills/wiki-lint/) | `/wiki-lint` flow definition |

---

## How a typical session looks

```
$ claude
> drop the new Karpathy blog clip into raw/

# you save the file to raw/karpathy-blog-2026-05.md, then:
> /wiki-ingest

# Claude reads, summarizes back 5 bullets, asks for go-ahead
> looks good, proceed

# Claude writes: wiki/Skills/Karpathy Blog 2026-05.md (source-summary)
#                wiki/Skills/Karpathy.md (new entity page if missing)
#                updates wiki/Skills/LLM Wiki Pattern.md (cites new source)
#                updates index.md + log.md
# 8 files touched, 14 wikilinks added

> /wiki-query how has Karpathy's thinking evolved since the original LLM Wiki essay?

# Claude reads index.md, opens both source-summaries, synthesizes
# answers with [[wikilinks]], asks "file this back as a wiki page?"
> yes, file under Skills

# Claude writes: wiki/Skills/Karpathy Thinking Evolution.md
# updates index.md + log.md

# ...session ends, you /exit
# SessionEnd hook runs: secret scan passes, autosave commits + pushes
# to origin/claude/autosave with message
# "chore(autosave): session save — 11 change(s): wiki/Skills/..."
```

The wiki grows. The maintenance cost is near zero. Bush would be pleased.

---

## Tips

- **One source at a time, stay involved.** Karpathy's recommendation. Batch-ingest with less supervision works, but the discussion at summary stage often catches misclassifications early.
- **File queries back aggressively.** Any synthesis you generated is wasted if it only lives in chat. The wiki compounds when explorations stick.
- **Lint monthly.** Catch orphans and stale claims before they decay further.
- **Don't write wiki pages by hand.** Let the LLM do it. If you must edit, do it through Claude in dialogue so it logs and propagates correctly.
- **Use Obsidian's graph view** to see the shape of your wiki — which pages are hubs, which are orphans.
- **Treat `CLAUDE.md` as a living document.** When you notice a pattern you want enforced consistently, add it to the schema instead of repeating yourself per session.

---

## Credits

- Pattern: [Andrej Karpathy, *LLM Wiki*](raw/karpathy-llm-wiki.md)
- Precedent: [Vannevar Bush, *As We May Think* (1945)](wiki/Skills/Memex.md) — the Memex
- IDE: [Obsidian](https://obsidian.md)
- Programmer: [Claude Code](https://claude.com/claude-code)
