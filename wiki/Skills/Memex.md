---
type: entity
tags: [knowledge-management, historical, vannevar-bush]
sources: [[raw/karpathy-llm-wiki]]
updated: 2026-05-27
---

# Memex

A hypothetical personal knowledge-store device proposed by **Vannevar Bush** in his 1945 *Atlantic* essay *"As We May Think."* The intellectual ancestor of the [[LLM Wiki Pattern]].

## Core idea (Bush, 1945)

A private, actively curated knowledge store where the **associative trails between documents** are as valuable as the documents themselves. Users would build trails of references — chains of related materials — that could be followed, shared, and extended.

## Why it matters here

Per [[raw/karpathy-llm-wiki|Karpathy]]:

> "Bush's vision was closer to this than to what the web became: private, actively curated, with the connections between documents as valuable as the documents themselves. The part he couldn't solve was who does the maintenance. The LLM handles that."

Memex was the right vision, blocked by a missing actor — the maintainer. LLMs fill that gap.

## Contrast with the web

| Memex (Bush's vision) | Web (what we got) |
|---|---|
| Private | Public |
| Actively curated | Crawled / aggregated |
| Associative trails (user-built) | Hyperlinks (author-asserted) |
| Maintenance is the bottleneck | Maintenance offloaded to authors |

## Modern incarnations

- **Personal wikis** (TiddlyWiki, Roam, Obsidian, Logseq) — manual associative trails. Same maintenance problem Bush hit.
- **[[LLM Wiki Pattern]]** — Bush's vision with the maintenance burden lifted by an LLM agent.

## Related

- [[LLM Wiki Pattern]] — the modern realization
- [[Karpathy LLM Wiki]] — source that surfaced the connection
- [[Compounding Knowledge vs RAG]] — what made this realization newly tractable
