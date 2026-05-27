---
type: concept
tags: [llm, knowledge-management, llm-wiki, rag, comparison]
sources: [[raw/karpathy-llm-wiki]]
updated: 2026-05-27
---

# Compounding Knowledge vs RAG

The core differentiator of the [[LLM Wiki Pattern]] over standard RAG (retrieval-augmented generation): knowledge is **compiled once and maintained**, not re-derived on every query. Defined in contrast by [[raw/karpathy-llm-wiki|Karpathy (2026)]].

## The two models

### Standard RAG (NotebookLM, ChatGPT uploads, most RAG systems)

```
[raw docs]  →  [vector index]  →  [retrieve chunks]  →  [LLM synthesizes]  →  [answer]
                                          ↑
                                  every query repeats this
```

- The LLM rediscovers knowledge from scratch on every question.
- A question requiring synthesis across 5 documents: the LLM has to find + piece together fragments **every time**.
- Nothing accumulates. The system at query 100 is no smarter than at query 1.

### LLM Wiki

```
[raw docs] → [LLM ingests once] → [wiki: summaries, entity pages, comparisons, index]
                                              ↑
                                  query reads the compiled wiki
                                              ↓
                                  good answers file back as new pages
                                              ↑
                                  wiki compounds with every query
```

- Knowledge is **integrated** on ingest, not on query.
- Cross-references already exist. Contradictions already flagged. Synthesis already reflects everything read.
- Wiki gets richer over time from both sources AND queries (file-back).

## Why the difference matters

| Dimension | RAG | LLM Wiki |
|---|---|---|
| Cost per query | Re-synthesize each time | Read existing compiled pages |
| Memory across queries | None | Persistent + compounding |
| Cross-references | Found on demand | Pre-built |
| Contradiction handling | Per-query | Flagged at ingest, surfaced at lint |
| Scale ceiling | Embedding index limits | Index file works to ~100 sources; add search beyond |
| User artifact | Black-box answers | Readable wiki the user can browse |
| Maintenance burden | Hidden in the index | Visible in the wiki + offloaded to LLM |

## What stays the same

Both still need:
- Source curation by the human
- LLM-driven synthesis
- Some retrieval step (RAG: embeddings; Wiki: read index.md first)

The Wiki model isn't strictly *better* at every query type — pure semantic search over very large corpora may still favor embedding retrieval. The Wiki shines when:
- The corpus is moderate scale (≤ ~hundreds of sources)
- The user wants to **browse** the knowledge, not just query it
- Cross-references and synthesis matter more than raw recall
- Maintenance over time is the goal

## Quote

> "The wiki is a persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything you've read." — [[raw/karpathy-llm-wiki|Karpathy]]

## Related

- [[LLM Wiki Pattern]] — full pattern
- [[Karpathy LLM Wiki]] — source-summary
- [[Memex]] — Bush's vision was closer to the Wiki model than to RAG
