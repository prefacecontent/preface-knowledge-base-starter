---
name: wiki-lint
description: >
  Health-check the wiki vault. Finds orphan pages, dead wikilinks, stale claims, missing
  cross-references, frontmatter gaps, and empty sections, then writes a short report.
  Triggers on: "lint", "health check", "clean up wiki", "check the wiki", "wiki maintenance",
  "find orphans", "wiki audit".
---

# wiki-lint: Wiki Health Check

Run lint after every 10-15 ingests, or monthly. Ask before auto-fixing anything. Write the report to `wiki/lint-report-YYYY-MM-DD.md`.

Use plain file operations: `Grep` to scan wikilinks and frontmatter across `wiki/`, `Read` to open individual pages, `Glob` to enumerate pages, `Write` for the report.

---

## Lint Checks

Work through these in order:

1. **Orphan pages** — wiki pages with no inbound wikilinks. They exist but nothing points to them. (Grep for the page's name in `[[...]]` across the vault; if no hits, it's an orphan.)
2. **Dead links** — wikilinks that reference a page that does not exist. (For each `[[Target]]`, confirm a file `Target.md` exists somewhere under `wiki/`.)
3. **Stale claims** — assertions on older pages that newer sources have contradicted or updated.
4. **Missing pages** — concepts or entities mentioned in multiple pages but lacking their own page.
5. **Missing cross-references** — entities mentioned in a page but not linked with `[[...]]`.
6. **Frontmatter gaps** — pages missing required fields (`type`, `title`, `created`, `updated`, `tags`).
7. **Empty sections** — headings with no content underneath.
8. **Stale index entries** — items in `wiki/index.md` pointing to renamed or deleted pages.

---

## Lint Report Format

Create at `wiki/lint-report-YYYY-MM-DD.md`:

```markdown
---
type: synthesis
title: "Lint Report YYYY-MM-DD"
tags: [lint]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# Lint Report: YYYY-MM-DD

## Summary
- Pages scanned: N
- Issues found: N
- Needs review: N

## Orphan Pages
- [[Page Name]]: no inbound links. Suggest: link from [[Related Page]] or delete.

## Dead Links
- [[Missing Page]]: referenced in [[Source Page]] but does not exist. Suggest: create a stub or remove the link.

## Missing Pages
- "concept name": mentioned in [[Page A]], [[Page B]], [[Page C]]. Suggest: create a concept page.

## Frontmatter Gaps
- [[Page Name]]: missing fields: tags, updated

## Stale Claims
- [[Page Name]]: claim "X" may conflict with newer source [[Newer Source]].

## Cross-Reference Gaps
- [[Entity Name]] mentioned in [[Page A]] without a wikilink.
```

---

## Naming Conventions

Enforce these during lint:

| Element | Convention | Example |
|---------|-----------|---------|
| Filenames | Title Case with spaces | `Machine Learning.md` |
| Folders | lowercase with dashes | `wiki/concepts/` |
| Tags | lowercase, hierarchical | `#domain/architecture` |
| Wikilinks | match filename exactly | `[[Machine Learning]]` |

Filenames must be unique across the vault. Wikilinks resolve without paths only if filenames are unique.

---

## Writing Style Check

Flag pages that violate the style guide:

- Not declarative present tense ("X basically does Y" instead of "X does Y").
- Missing source citations where claims are made.
- Uncertainty not flagged with `> [!gap]`.
- Contradictions not flagged with `> [!contradiction]`.

---

## Before Auto-Fixing

Always show the lint report first. Ask: "Should I fix these automatically, or do you want to review each one?"

Safe to auto-fix:
- Adding missing frontmatter fields with placeholder values.
- Creating stub pages for missing entities.
- Adding wikilinks for unlinked mentions.

Needs review before fixing:
- Deleting orphan pages (they might be intentionally isolated).
- Resolving contradictions (requires human judgment).
- Merging duplicate pages.
