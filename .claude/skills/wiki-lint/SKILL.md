---
name: wiki-lint
description: Health-check the LLM Wiki per the schema in CLAUDE.md. Surfaces contradictions, stale claims, orphan pages, missing entities, broken wikilinks, and topic gaps. Produces a dated lint report at the vault root and logs it. Does NOT fix issues unilaterally — surfaces them for user direction. Tells the user the wiki is empty if it has not been bootstrapped. Triggers lint the wiki, wiki health check, find orphans, find contradictions, audit the wiki, what's missing in the wiki, vault cleanup, fix broken links.
---

# /wiki-lint

Run a full wiki health pass per `CLAUDE.md`. **Surface issues, do NOT fix them** unless I direct.

## Step 0 — Bootstrap check

- If `wiki/` does not exist OR is empty → tell me "Wiki is empty. Nothing to lint. Ingest a source first." Exit.
- If `index.md` is missing → create a starter (see `/wiki-ingest` Step 0) before scanning, so the report has somewhere to link from.
- If `log.md` is missing → create a starter so this lint pass gets logged.

## Inputs

`$ARGUMENTS` — optional scope: `entities`, `orphans`, `links`, `contradictions`, `gaps`, `stale`, or empty for all.

## Checks

1. **Contradictions.** Pages with disagreeing claims about the same entity/concept. Flag with: both page links, the disagreeing quotes, suggested resolution (which source is newer / more authoritative).
2. **Stale claims.** Pages whose `updated:` is older than the most recent ingest touching the same entity. Likely missed a cascade update.
3. **Orphan pages.** Grep inbound `[[Page]]` references across the vault. Any page with zero inbound links from another wiki page = orphan. (`index.md` counts as inbound.)
4. **Missing entities.** Concepts / people / products mentioned ≥3× in plain text (not as wikilinks) but lacking their own page.
5. **Broken wikilinks.** `[[Page Name]]` pointing at a non-existent file.
6. **Topic gaps.** Topics with high mention count but thin coverage (1-2 sources). Suggest a web search or source to ingest.
7. **Raw source coverage.** Files in `raw/` not referenced by any wiki page → unprocessed sources. List them.

## Output

1. **Write a lint report page** at the vault root: `lint-YYYY-MM-DD.md`. Sections one per check above. Each issue gets: type, severity (high/med/low), affected pages (wikilinks), suggested action.
2. **Append to `log.md`:**
   ```
   ## [YYYY-MM-DD] lint | N issues — see [[lint-YYYY-MM-DD]]
   ```
3. **Summarize in chat:** counts per category, top 5 highest-severity items, then ask: "Want me to start fixing? Which categories first?"

## Hard rules

- **Read-only by default.** No writes to wiki pages during lint (except the report itself + log).
- **Severity must be defensible.** High = factual contradiction or broken link in heavily-cited page. Med = orphan with substantive content. Low = stylistic / missing-frontmatter issues.
- **Don't suggest fixes you can't justify.** If unsure between two resolutions for a contradiction, list both and let me pick.
