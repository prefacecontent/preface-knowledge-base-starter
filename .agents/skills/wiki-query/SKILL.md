---
name: wiki-query
description: "Answer questions using the wiki vault. Reads the hot cache first, then the index, then relevant pages. Synthesizes answers with citations and optionally files good answers back as wiki pages. Triggers on: what do you know about, query:, what is, explain, summarize, find in wiki, search the wiki, based on the wiki, wiki query."
allowed-tools: Read Glob Grep
---

# wiki-query: Query the Wiki

The wiki has already done the synthesis work. Read strategically, answer precisely, and file good answers back so the knowledge compounds.

Use plain file operations: `Read` to open pages, `Grep`/`Glob` to find them.

---

## Query Modes

Three depths. Choose based on the question's complexity.

| Mode | Trigger | Reads | Best for |
|------|---------|-------|----------|
| **Quick** | `query quick: ...` or simple factual Q | hot.md + index.md only | "What is X?", date lookups, quick facts |
| **Standard** | default (no flag) | hot.md + index + 3-5 pages | most questions |
| **Deep** | `query deep: ...` or "thorough", "comprehensive" | full wiki + optional web | "Compare A vs B across everything", synthesis, gap analysis |

---

## Quick Mode

Use when the answer is likely in the hot cache or index summary.

1. Read `wiki/hot.md`. If it answers the question, respond immediately.
2. If not, read `wiki/index.md`. Scan descriptions for the answer.
3. If found in the index summary, respond and do not open any pages.
4. If not found, say "Not in quick cache. Run as standard query?"

Do not open individual wiki pages in quick mode.

---

## Standard Query Workflow

1. **Read** `wiki/hot.md` first. It may already have the answer or directly relevant context.
2. **Read** `wiki/index.md` to find the most relevant pages (scan titles and descriptions). Use `Grep` across `wiki/` to locate specific content.
3. **Read** those pages. Follow wikilinks to depth-2 for key entities. No deeper.
4. **Synthesize** the answer in chat. Cite sources with wikilinks: `(Source: [[Page Name]])`.
5. **Offer to file** the answer: "This seems worth keeping. Should I save it to `wiki/synthesis/`?"
6. If the question reveals a **gap**: say "I don't have enough on X. Want to find a source?"

---

## Deep Mode

Use for synthesis questions, comparisons, or "tell me everything about X."

1. Read `wiki/hot.md` and `wiki/index.md`.
2. Identify all relevant pages (concepts, entities, sources, comparisons, synthesis). Use `Grep` to be thorough.
3. Read every relevant page. No skipping.
4. If wiki coverage is thin, offer to supplement with web search.
5. Synthesize a comprehensive answer with full citations.
6. Always file the result back into `wiki/synthesis/`. Deep answers are too valuable to lose.

---

## Token Discipline

Read the minimum needed:

| Start with | When to stop |
|------------|--------------|
| hot.md | if it has the answer |
| index.md | if you can identify 3-5 relevant pages |
| 3-5 wiki pages | usually sufficient |
| 10+ wiki pages | only for synthesis across the entire wiki |

If hot.md has the answer, respond without reading further.

---

## Index Format Reference

The master index (`wiki/index.md`) is organised by section:

```markdown
## Entities
- [[Entity Name]]: role (first seen: [[Source]])

## Concepts
- [[Concept Name]]: definition

## Sources
- [[Source Title]]: author, date, type

## Synthesis
- [[Synthesis Title]]: answer summary
```

Scan the section headers first to decide which sections to read.

---

## Filing Answers Back

Good answers compound into the wiki. Don't let insights disappear into chat history.

When filing a synthesis answer, write it to `wiki/synthesis/<Title>.md` with frontmatter:

```yaml
---
type: synthesis
title: "Short descriptive title"
question: "The exact query as asked."
tags: [synthesis, <domain>]
created: YYYY-MM-DD
updated: YYYY-MM-DD
related:
  - "[[Page referenced in answer]]"
sources:
  - "[[Relevant Source]]"
---
```

Then write the answer as the page body. Include citations. Link every mentioned concept or entity.

After filing, add an entry to `wiki/index.md` under Synthesis and append to `wiki/log.md`.

---

## Gap Handling

If the question cannot be answered from the wiki:

1. Say clearly: "I don't have enough in the wiki to answer this well."
2. Identify the specific gap: "I have nothing on [subtopic]."
3. Suggest: "Want to find a source on this? I can ingest one with wiki-ingest."
4. Do not fabricate. Do not answer from training data if the question is about this wiki's specific domain.
