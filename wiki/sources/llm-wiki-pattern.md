---
title: "LLM Wiki Pattern"
type: source
created: 2026-04-07
updated: 2026-04-07
tags: [meta, knowledge-management, llm, methodology]
sources: [llm-wiki-pattern.md]
---

# LLM Wiki Pattern

**Type:** Idea file / methodology document
**Author:** Unknown (community-sourced pattern)

## Summary

A design pattern for building personal knowledge bases using LLMs. Instead of RAG (re-deriving answers from raw documents at query time), the LLM incrementally builds and maintains a persistent wiki — a structured, interlinked collection of markdown files that compounds knowledge over time.

## Core Insight

> The tedious part of maintaining a knowledge base is the bookkeeping — cross-references, summaries, consistency. Humans abandon wikis because maintenance burden grows faster than value. LLMs handle this at near-zero cost.

## Architecture

Three layers:
1. **Raw sources** — Immutable source documents. The human curates these.
2. **The wiki** — LLM-generated and maintained markdown files. Summaries, entity pages, concept pages, cross-references.
3. **The schema** — Configuration file (e.g., CLAUDE.md) defining structure, conventions, and workflows.

## Four Operations

1. **Ingest** — Process a new source into the wiki. Read, summarize, cross-reference, update entities and concepts.
2. **Query** — Ask questions against the wiki (not raw sources). File good answers as new pages.
3. **Lint** — Health-check for contradictions, orphans, stale claims, missing pages.
4. **Evolve** — Update the schema as conventions change.

## Historical Connection

Related to Vannevar Bush's **Memex** (1945) — a personal knowledge store with associative trails between documents. Bush's vision couldn't solve the maintenance problem. LLMs solve it.

## Relevance

This is the founding methodology of this wiki. Every interaction follows the pattern described here, instantiated through our CLAUDE.md schema.

## Mentioned In

- [[Wiki Overview]]
