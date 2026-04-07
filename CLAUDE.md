# LLM Wiki — Schema & Operating Manual

This is the schema file for an LLM-maintained personal wiki (second brain). It governs how the LLM agent reads, writes, and maintains the wiki. Every interaction in this project follows these rules.

---

## Directory Structure

```
.
├── CLAUDE.md              # This file — the schema (LLM + human co-evolve)
├── raw/                   # Immutable source documents (human-curated)
│   ├── assets/            # Downloaded images, PDFs, attachments
│   └── ...                # Articles, papers, notes, transcripts, etc.
├── wiki/                  # LLM-generated and maintained wiki pages
│   ├── index.md           # Master catalog of all wiki pages
│   ├── log.md             # Chronological record of all operations
│   ├── overview.md        # High-level synthesis of the entire wiki
│   ├── sources/           # One summary page per ingested source
│   ├── entities/          # Pages for people, orgs, tools, projects
│   ├── concepts/          # Pages for ideas, frameworks, theories, topics
│   └── analyses/          # Comparisons, syntheses, query results filed as pages
└── .obsidian/             # Obsidian config (not managed by LLM)
```

## Ownership Rules

| Layer | Owner | Mutability |
|-------|-------|------------|
| `raw/` | Human | Immutable — LLM reads but NEVER modifies |
| `wiki/` | LLM | LLM creates, updates, and deletes pages freely |
| `CLAUDE.md` | Both | Human and LLM co-evolve collaboratively |

---

## Page Conventions

### Frontmatter

Every wiki page MUST have YAML frontmatter:

```yaml
---
title: "Page Title"
type: source | entity | concept | analysis | overview
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2]
sources: [filename1.md, filename2.md]  # raw/ sources this page draws from
---
```

### Wikilinks

Use Obsidian-style wikilinks for all cross-references: `[[Page Title]]`. This enables Obsidian's graph view and backlink tracking.

### File Naming

- All lowercase, hyphens for spaces: `machine-learning.md`, `daniel-kahneman.md`
- Source summaries match the raw filename: if raw file is `atomic-habits.md`, summary is `wiki/sources/atomic-habits.md`
- No spaces in filenames. No special characters except hyphens.

### Page Types

**Source pages** (`wiki/sources/`): One per ingested source. Contains:
- Metadata (author, date, URL if applicable)
- Summary (2-5 paragraphs)
- Key takeaways (bulleted)
- Notable quotes (if any)
- Links to entity/concept pages created or updated from this source

**Entity pages** (`wiki/entities/`): People, organizations, tools, projects, places. Contains:
- Description
- Key facts
- Relevance to the wiki's themes
- Links to source pages and concept pages

**Concept pages** (`wiki/concepts/`): Ideas, frameworks, theories, topics, mental models. Contains:
- Definition / explanation
- Key aspects or sub-topics
- Connections to other concepts
- Sources that discuss this concept

**Analysis pages** (`wiki/analyses/`): Filed query results, comparisons, syntheses. Contains:
- The question or framing
- The analysis
- Sources and pages consulted
- Conclusions or open questions

**Overview** (`wiki/overview.md`): High-level synthesis of the entire wiki. Updated periodically. Describes the major themes, key entities, and how they connect.

---

## Operations

### 1. INGEST

Triggered when the human adds a new source to `raw/` and asks the LLM to process it.

**Workflow (collaborative mode):**

1. **Read** the raw source fully.
2. **Discuss** key takeaways with the human. Ask: "Here's what I found most notable — what do you want to emphasize or explore?"
3. **Create** a source summary page in `wiki/sources/`.
4. **Create or update** entity pages in `wiki/entities/` for any people, orgs, tools, or projects mentioned.
5. **Create or update** concept pages in `wiki/concepts/` for key ideas, frameworks, or topics.
6. **Update** `wiki/index.md` — add new pages, update descriptions of modified pages.
7. **Update** `wiki/overview.md` if the source meaningfully changes the big picture.
8. **Log** the ingestion in `wiki/log.md`.
9. **Report** to the human: list all pages created/updated, note any contradictions with existing wiki content, suggest follow-up questions or sources.

**Rules:**
- If new information contradicts existing wiki content, flag it explicitly. Update the wiki to reflect the contradiction with a `> [!warning]` callout, don't silently overwrite.
- A single source may touch 5-20 wiki pages. That's normal and expected.
- Always link back to the source page from any page it influences.
- Use `> [!info]` callouts for editorial notes (e.g., "This claim is unsupported by other sources in the wiki").

### 2. QUERY

Triggered when the human asks a question against the wiki.

**Workflow:**

1. **Read** `wiki/index.md` to identify relevant pages.
2. **Read** the relevant wiki pages (not raw sources — the wiki IS the compiled knowledge).
3. **Synthesize** an answer with `[[wikilinks]]` to pages consulted.
4. **Optionally file** the answer as an analysis page in `wiki/analyses/` if the human confirms it's worth keeping.
5. **Log** the query in `wiki/log.md`.

**Rules:**
- Cite wiki pages, not raw sources directly. The wiki is the interface.
- If the wiki doesn't contain enough information, say so and suggest what sources could fill the gap.
- If a question reveals a gap or contradiction in the wiki, flag it.

### 3. LINT

Triggered when the human asks for a health check, or proactively suggested by the LLM.

**Checks:**
- [ ] Orphan pages (no inbound links from other wiki pages)
- [ ] Stub pages (created but underdeveloped)
- [ ] Contradictions between pages
- [ ] Stale claims superseded by newer sources
- [ ] Missing pages (concepts/entities mentioned but lacking their own page)
- [ ] Missing cross-references between related pages
- [ ] Index accuracy (all pages listed, descriptions current)
- [ ] Broken wikilinks

**Output:** A report with findings and suggested actions. Apply fixes with human approval.

### 4. EVOLVE

The human or LLM can propose changes to this schema file (CLAUDE.md) at any time. Examples:
- Adding a new page type
- Changing the ingestion workflow
- Adding conventions for a specific domain
- Adjusting frontmatter fields

---

## Index & Log

### index.md

The master catalog. Structure:

```markdown
# Wiki Index

## Sources
- [[source-page]] — one-line summary (YYYY-MM-DD)

## Entities
- [[entity-page]] — one-line description

## Concepts
- [[concept-page]] — one-line description

## Analyses
- [[analysis-page]] — one-line description (YYYY-MM-DD)
```

Updated on every ingest and whenever pages are created/modified.

### log.md

Append-only chronological record. Each entry:

```markdown
## [YYYY-MM-DD] operation | Title
- **Operation:** ingest / query / lint / evolve
- **Details:** what was done
- **Pages touched:** [[page1]], [[page2]], ...
```

The log is never edited retroactively — only appended.

---

## Interaction Protocol

Every interaction in this project follows one of the four operations above (ingest, query, lint, evolve). At the start of each interaction:

1. The LLM determines which operation applies.
2. The LLM follows the corresponding workflow.
3. The LLM updates index.md and log.md as specified.

If an interaction doesn't fit any operation (e.g., casual conversation), the LLM still responds helpfully but does not modify the wiki.

---

## Tips for the Human

- **Add sources to `raw/`** and tell me to ingest them. One at a time is best for quality.
- **Ask questions freely** — I'll answer from the wiki and suggest filing good answers as analysis pages.
- **Request a lint** periodically to keep the wiki healthy.
- **Browse in Obsidian** — use graph view, backlinks, and Dataview queries to explore.
- **Propose schema changes** anytime something isn't working. This file evolves with us.
