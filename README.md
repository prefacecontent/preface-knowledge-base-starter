---
title: "Preface AI Knowledge Base Starter"
status: template
created: 2026-06-16
---

# Preface AI Knowledge Base Starter

A ready-to-use scaffold for building your own AI knowledge base: a digital replica that holds
your knowledge and reasons in your voice. It is a plain folder of markdown, an Obsidian vault,
and a set of agent skills. No database, no lock-in.

## What is inside

```
your-brain/
  raw/                  source material you drop in (immutable)
  wiki/
    sources/            one page per ingested source
    entities/           people, companies, assets
    concepts/           ideas, frameworks, mental models
    comparisons/        structured A-vs-B comparisons
    synthesis/          joined-up playbooks and analyses
    sessions/           saved conversations and session notes
    context/            WHO THIS BRAIN IS  (the identity layer)
    index.md log.md hot.md overview.md
  skills/               identity, wiki-ingest, wiki-query, wiki-lint, save, autoresearch, obsidian-markdown, think
  AGENTS.md             how the agent should behave
```

## How to use it

1. Open this folder in your agent (OpenCode, Claude Code, Codex). The course uses OpenCode.
2. Drop your sources into `raw/`: any files (PDFs, slides, notes, exports), or ask the agent to research a topic and write them there.
3. Ask the agent to run `wiki-ingest`. It builds linked pages in `wiki/`.
4. Open this folder in Obsidian with "Open folder as vault" (do not create a new vault). The graph view, accent colour and excluded scaffolding are already configured.
5. Run the `identity` skill to fill `wiki/context/`. It interviews you section by section and writes the files for you (it can also web-search to fill gaps, with your permission). This is what turns a knowledge base into a digital replica.
6. Ask questions with `wiki-query`. Answers come with sources, in your voice.

The context layer in `wiki/context/` is what turns a knowledge base into a digital replica. The `identity` skill is how you fill it. Spend time on it.
