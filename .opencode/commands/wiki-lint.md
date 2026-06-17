---
description: Health check the wiki — finds orphan pages, dead wikilinks, stale claims, missing cross-references, frontmatter gaps, and empty sections.
---

Read the `wiki-lint` skill. Then run the full lint check suite in order:

1. Orphan pages (no inbound links)
2. Dead wikilinks (targets that don't exist)
3. Stale claims (dated assertions older than 90 days without re-validation)
4. Missing cross-references (related pages not linked)
5. Frontmatter gaps (pages missing required fields)
6. Empty sections (headings with no body)

Write a lint report to `wiki/lint-report-YYYY-MM-DD.md`. Ask before auto-fixing anything.
