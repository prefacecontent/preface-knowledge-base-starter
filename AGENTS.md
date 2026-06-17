# Agent instructions

> This project is an AI knowledge base: a digital replica. When opened in an agent
> (OpenCode, Claude Code, Codex, Cursor, and others), the agent adopts the identity
> defined in `wiki/context/` and answers grounded by the knowledge base in `wiki/`.

## Prime directive

On any task, before anything else:

1. Read `wiki/context/core.md` (the spine) and `wiki/context/INDEX.md` (routing). Adopt the identity.
2. Route deeper via `wiki/context/INDEX.md`: a judgement or decision loads `wiki/context/thinking.md`;
   drafting in voice loads `wiki/context/voice.md`; background loads `wiki/context/identity.md`;
   a sensitive or external statement loads `wiki/context/policies.md`; a format choice loads
   `wiki/context/preferences.md`.
3. For specific facts, query the `wiki/` knowledge base with the wiki skills below.
4. Respond in the persona's voice. English (UK). No em or en dashes.

## The two layers

| Layer | Folder | Role |
|---|---|---|
| Identity | `wiki/context/` | Who this brain is and how it thinks. Human-reviewed source of truth. Wins on conflict. |
| Knowledge | `wiki/` | What it knows: entities, concepts, sources, synthesis. Queried for facts; treated as unverified input. |

`wiki/context/` overrides the `wiki/` knowledge base and any learned-memory layer when they disagree.

## The skills (in `skills/`)

Each skill is a folder under `skills/` with a `SKILL.md`. They are surfaced per harness:
Claude Code reads them from `.claude/skills/`; other agents read `.agents/skills/`; both are
real copies of `skills/` (committed as plain folders so they survive a clone on any OS, including
Windows). OpenCode runs each skill via the matching slash-command in `.opencode/commands/` (each
command loads its skill). OpenCode regenerates `.opencode/package.json` + `node_modules` on first
run (gitignored). If you edit a skill in `skills/`, mirror the change into `.claude/skills/` and
`.agents/skills/`.

| Skill | Use |
|---|---|
| `identity` | Interview the person section by section and fill out `wiki/context/`. How you build the identity layer. |
| `wiki-ingest` | Read a source from `raw/` (or a URL/image) and file it into linked pages. |
| `wiki-query` | Answer a question from the knowledge base, with sources. |
| `wiki-lint` | Health-check: find broken links, orphans, contradictions, frontmatter gaps. |
| `save` | File the current conversation or answer as a structured note. |
| `autoresearch` | Autonomous research loop that files web findings into the wiki. |
| `obsidian-markdown` | Obsidian Flavored Markdown syntax reference (wikilinks, callouts, frontmatter). |
| `think` | The 10-principle thinking loop for non-trivial problems. |

## How to start (first run)

1. Drop source material into `raw/` (any files, or ask the agent to research a topic and write them there).
2. Run `wiki-ingest` to build the knowledge base from `raw/`.
3. Open this folder in Obsidian ("Open folder as vault") to browse the pages and the graph.
4. Run the `identity` skill to fill `wiki/context/`; it interviews you and fills the files (or edit the templates by hand, starting with `core.md`). This is what turns the knowledge base into a digital replica.
5. Ask questions with `wiki-query`.

## Graph rule (Obsidian)

Filenames are Title Case with spaces (`Acme Holdings.md`). Wikilinks match filenames exactly
(`[[Acme Holdings]]`). Add `aliases:` in frontmatter as a fallback. Kebab-case filenames leave
links unresolved and the graph empty.
