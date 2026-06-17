---
name: save
description: >
  Save the current conversation, answer, or insight into the wiki vault as a structured note.
  Analyzes the chat, picks the right note type, writes frontmatter, files it in the correct
  wiki folder, and updates the index, log, and hot cache.
  Triggers on: "save this", "save that answer", "/save", "file this", "save to wiki",
  "save this session", "file this conversation", "keep this", "save this analysis",
  "add this to the wiki".
allowed-tools: Read Write Edit Glob Grep
---

# save: File Conversations Into the Wiki

Good answers and insights shouldn't disappear into chat history. This skill takes what was just discussed and files it as a permanent wiki page.

The wiki compounds. Save often.

Use plain file operations: `Read`/`Grep` to check for an existing page, `Write`/`Edit` to create or update.

---

## Note Type Decision

Determine the best type from the conversation content:

| Type | Folder | Use when |
|------|--------|----------|
| synthesis | `wiki/synthesis/` | Multi-step analysis, comparison, or answer to a specific question |
| concept | `wiki/concepts/` | Explaining or defining an idea, pattern, or framework |
| source | `wiki/sources/` | Summary of external material discussed in the session |
| comparison | `wiki/comparisons/` | A structured A-vs-B comparison |
| session | `wiki/sessions/` | Full session summary: captures everything discussed |

If the user specifies a type, use that. Otherwise pick the best fit. When in doubt, use `synthesis`.

---

## Save Workflow

1. **Scan** the current conversation. Identify the most valuable content to preserve.
2. **Ask** (if not already named): "What should I call this note?" Keep the name short and descriptive. Filenames are Title Case with spaces.
3. **Determine** the note type using the table above.
4. **Extract** all relevant content from the conversation. Rewrite it in declarative present tense (not "the user asked" but the actual content itself).
5. **Check for an existing page** with the same name (`Grep`/`Glob`). If one exists, ASK before overwriting — prefer updating it.
6. **Create** the note in the chosen folder with full frontmatter (template below).
7. **Collect links**: identify any wiki pages mentioned in the conversation and add them to `related` in frontmatter. Link them inline with `[[wikilinks]]` too.
8. **Update** `wiki/index.md`. Add the new entry at the top of the relevant section.
9. **Append** to `wiki/log.md`. New entry at the TOP:
   ```
   ## [YYYY-MM-DD] save | Note Title
   - Type: [note type]
   - Location: wiki/[folder]/Note Title.md
   - From: conversation on [brief topic description]
   ```
10. **Update** `wiki/hot.md` to reflect the new addition.
11. **Confirm**: "Saved as [[Note Title]] in wiki/[folder]/."

---

## Frontmatter Template

```yaml
---
type: <synthesis|concept|source|comparison|session>
title: "Note Title"
tags:
  - <relevant-tag>
created: YYYY-MM-DD
updated: YYYY-MM-DD
related:
  - "[[Any Wiki Page Mentioned]]"
sources:
  - "[[raw-source-if-applicable]]"
---
```

For `synthesis` answering a specific question, also add:
```yaml
question: "The original query as asked."
```

---

## Writing Style

- Declarative, present tense. Write the knowledge, not the conversation.
- Not: "The user asked about X and I explained..."
- Yes: "X works by doing Y. The key insight is Z."
- Include all relevant context. Future sessions should be able to read this page cold.
- Link every mentioned concept, entity, or wiki page with wikilinks.
- Cite sources where applicable: `(Source: [[Page]])`.

---

## What to Save vs. Skip

Save:
- Non-obvious insights or synthesis
- Decisions with rationale
- Analyses that took significant effort
- Comparisons likely to be referenced again
- Research findings

Skip:
- Mechanical Q&A (lookup questions with obvious answers)
- Setup steps already documented elsewhere
- Temporary debugging with no lasting insight
- Anything already in the wiki

If it's already in the wiki, update the existing page instead of creating a duplicate.
