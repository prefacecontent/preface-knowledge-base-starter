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
    context/            WHO THIS BRAIN IS  (fill these in first)
    index.md log.md hot.md overview.md
  skills/               identity, wiki-ingest, wiki-query, wiki-lint, save, autoresearch, obsidian-markdown, think
  AGENTS.md             how the agent should behave
```

## How to use it

1. Open this folder in Obsidian with "Open folder as vault" (do not create a new vault). The graph view, accent colour and excluded scaffolding are already configured. Also open it in your agent (OpenCode, Claude Code, Codex).
2. Fill in `wiki/context/` by running the `identity` skill. It interviews you section by section and writes the files for you (it can also web-search to fill gaps, with your permission). You can do it in passes, and edit the templates by hand if you prefer.
3. Drop your sources into `raw/`.
4. Ask the agent to run `wiki-ingest`. It builds linked pages in `wiki/`.
5. Ask questions with `wiki-query`. Answers come with sources, in your voice.

The context layer in `wiki/context/` is what turns a knowledge base into a digital replica. The `identity` skill is how you fill it. Spend time on it.
