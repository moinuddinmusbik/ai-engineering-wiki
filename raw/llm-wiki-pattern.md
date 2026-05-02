# LLM Wiki

A pattern for building personal knowledge bases using LLMs.

## The core idea

Most people's experience with LLMs and documents looks like RAG: you upload a collection of files, the LLM retrieves relevant chunks at query time, and generates an answer. This works, but the LLM is rediscovering knowledge from scratch on every question. There's no accumulation. Ask a subtle question that requires synthesizing five documents, and the LLM has to find and piece together the relevant fragments every time. Nothing is built up. NotebookLM, ChatGPT file uploads, and most RAG systems work this way.

The idea here is different. Instead of just retrieving from raw documents at query time, the LLM incrementally builds and maintains a persistent wiki — a structured, interlinked collection of markdown files that sits between you and the raw sources. When you add a new source, the LLM doesn't just index it for later retrieval. It reads it, extracts the key information, and integrates it into the existing wiki — updating entity pages, revising topic summaries, noting where new data contradicts old claims, strengthening or challenging the evolving synthesis. The knowledge is compiled once and then kept current, not re-derived on every query.

This is the key difference: the wiki is a persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything you've read. The wiki keeps getting richer with every source you add and every question you ask.

You never (or rarely) write the wiki yourself — the LLM writes and maintains all of it. You're in charge of sourcing, exploration, and asking the right questions. The LLM does all the grunt work — the summarizing, cross-referencing, filing, and bookkeeping that makes a knowledge base actually useful over time.

## Architecture

Three layers:

- **Raw sources** — your curated collection of source documents. Immutable — the LLM reads from them but never modifies them. This is your source of truth.
- **The wiki** — a directory of LLM-generated markdown files. The LLM owns this layer entirely.
- **The schema** — a document (e.g. CLAUDE.md) that tells the LLM how the wiki is structured.

## Operations

- **Ingest**: Process a new source into the wiki. Reads, summarizes, cross-references, updates entities and concepts.
- **Query**: Ask questions against the wiki. The LLM reads wiki pages (not raw sources) to synthesize answers.
- **Lint**: Health-check the wiki for contradictions, orphans, stale claims, missing pages.
- **Evolve**: Update the schema as conventions change.

## Key insight

The tedious part of maintaining a knowledge base is the bookkeeping — cross-references, summaries, consistency. Humans abandon wikis because maintenance burden grows faster than value. LLMs handle this at near-zero cost. The human curates sources, directs analysis, and asks questions. The LLM does everything else.

Related to Vannevar Bush's Memex (1945) — a personal knowledge store with associative trails. The part Bush couldn't solve was who does the maintenance. The LLM handles that.
