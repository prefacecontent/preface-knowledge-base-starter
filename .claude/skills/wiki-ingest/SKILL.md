---
name: wiki-ingest
description: "Ingest sources into the wiki vault. Reads a source, extracts entities and concepts, creates or updates wiki pages, cross-references them, updates the index, and logs the operation. Supports files, URLs, images, and batch mode. Triggers on: ingest, process this source, add this to the wiki, read and file this, batch ingest, ingest all of these, ingest this url."
---

# wiki-ingest: Source Ingestion

Read the source. Write the wiki. Cross-reference everything. A single source typically touches several wiki pages.

Use plain file operations throughout: `Read` to read sources and existing pages, `Grep`/`Glob` to find existing pages before creating new ones, and `Write`/`Edit` to create and update pages.

**Syntax standard**: Write all wiki pages in Obsidian Flavored Markdown — wikilinks as `[[Note Name]]`, callouts as `> [!type] Title`, embeds as `![[file]]`, properties as YAML frontmatter. See the `obsidian-markdown` skill for the full syntax reference.

---

## Where pages go (fixed folders)

| Page type | Folder |
|---|---|
| source | `wiki/sources/` |
| entity | `wiki/entities/` |
| concept | `wiki/concepts/` |
| comparison | `wiki/comparisons/` |
| synthesis | `wiki/synthesis/` |

Filenames are **Title Case with spaces** (e.g. `Andrej Karpathy.md`). Wikilinks must match filenames exactly (`[[Andrej Karpathy]]`). Page templates live in `_templates/`.

---

## URL Ingestion

Trigger: user passes a URL starting with `https://`.

1. **Fetch** the page using WebFetch.
2. **Derive a slug** from the URL path (last segment, lowercased, spaces → hyphens, strip query strings).
3. **Save** the fetched content to `raw/[slug]-[YYYY-MM-DD].md` with a frontmatter header:
   ```markdown
   ---
   source_url: [url]
   fetched: [YYYY-MM-DD]
   ---
   ```
4. Proceed with **Single Source Ingest** below (the file is now in `raw/`).

---

## Image / Vision Ingestion

Trigger: user passes an image file path (`.png`, `.jpg`, `.jpeg`, `.gif`, `.webp`, `.svg`, `.avif`).

1. **Read** the image with the Read tool (images are processed natively).
2. **Describe** the contents: extract all text (OCR), identify key concepts, entities, diagrams, and data.
3. **Save** the description to `raw/[slug]-[YYYY-MM-DD].md`:
   ```markdown
   ---
   source_type: image
   original_file: [original path]
   fetched: YYYY-MM-DD
   ---
   # Image: [slug]

   [Full description of image contents, transcribed text, entities visible, etc.]
   ```
4. Proceed with **Single Source Ingest** on the saved description file.

Use cases: whiteboard photos, screenshots, diagrams, infographics, document scans.

---

## Single Source Ingest

Trigger: user drops a file into `raw/` or pastes content.

1. **Read** the source completely. Do not skim.
2. **Discuss** key takeaways with the user. Ask: "What should I emphasize? How granular?" Skip this if the user says "just ingest it."
3. **Create** a source summary in `wiki/sources/` using `_templates/source.md`. Full frontmatter.
4. **Create or update** entity pages in `wiki/entities/` for every person, org, product, and repo mentioned. One page per entity. Use `_templates/entity.md`.
5. **Create or update** concept pages in `wiki/concepts/` for significant ideas and frameworks. Use `_templates/concept.md`.
6. **Cross-link** everything with `[[wikilinks]]`. The source page links the entities and concepts it introduces; each entity/concept links back to the source.
7. **Update** `wiki/index.md`. Add entries for all new pages under the right sections.
8. **Update** `wiki/overview.md` if the big picture changed.
9. **Update** `wiki/hot.md` with this ingest's context (so the next session starts fast).
10. **Append** to `wiki/log.md` (new entries at the TOP):
    ```markdown
    ## [YYYY-MM-DD] ingest | Source Title
    - Source: `raw/filename.md`
    - Summary: [[Source Title]]
    - Pages created: [[Page 1]], [[Page 2]]
    - Pages updated: [[Page 3]], [[Page 4]]
    - Key insight: One sentence on what is new.
    ```
11. **Check for contradictions.** If new info conflicts with an existing page, add `> [!contradiction]` callouts on both pages (see below).

---

## Batch Ingest

Trigger: user drops multiple files or says "ingest all of these."

1. List all files to process. Confirm with the user before starting.
2. Process each source following the single-ingest flow. Defer cross-referencing *between* the new sources until step 3.
3. After all sources: do a cross-reference pass. Look for connections between the newly ingested sources.
4. Update index, hot cache, and log once at the end (not per-source).
5. Report: "Processed N sources. Created X pages, updated Y pages. Here are the key connections I found."

Batch ingest is less interactive. For 30+ sources, expect significant processing time. Check in after every 10 sources.

---

## Frontmatter

Every wiki page carries YAML frontmatter:

```yaml
---
type: <source|entity|concept|comparison|synthesis>
title: "Page Title"
tags:
  - <relevant-tag>
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - "[[Source Page]]"
---
```

Dates as `YYYY-MM-DD`. Wikilinks in YAML must be quoted. Lists use the `- item` form. See the `obsidian-markdown` skill for the full rules.

---

## Contradictions

When new info contradicts an existing wiki page, do NOT silently overwrite. Flag both sides and let the user decide.

On the existing page, add:
```markdown
> [!contradiction] Conflict with [[New Source]]
> [[Existing Page]] claims X. [[New Source]] says Y.
> Needs resolution. Check dates, context, and primary sources.
```

On the new source summary, reference it:
```markdown
> [!contradiction] Contradicts [[Existing Page]]
> This source says Y, but the existing wiki says X. See [[Existing Page]].
```

---

## Context Window Discipline

- Read `wiki/hot.md` first. If it has the relevant context, don't re-read full pages.
- Read `wiki/index.md` to find existing pages before creating new ones (use Grep to search).
- Read only 3-5 existing pages per ingest. If you need 10+, you are reading too broadly.
- Keep wiki pages short — roughly 100-300 lines. If a page grows past that, split it.

---

## What Not to Do

- **Source files under `raw/` are immutable.** Never modify the files users drop there.
- Do not create duplicate pages. Always check the index and Grep before creating.
- Do not skip the log entry. Every ingest must be recorded.
- Do not skip the hot-cache update. It is what keeps future sessions fast.
