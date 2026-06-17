---
description: Ingest a source (file, URL, or batch) into the wiki — extracts entities and concepts, creates or updates pages, cross-references, and logs the operation.
---

Read the `wiki-ingest` skill. Then run the ingestion workflow:

Usage:
- `/wiki-ingest <path-or-url>` — ingest a single source.
- `/wiki-ingest` — if files are staged in `raw/`, offer to ingest them.
- `/wiki-ingest batch` — ingest all staged files in `raw/`.

A single source typically touches 8-15 wiki pages. After ingestion, update `wiki/index.md`, `wiki/log.md`, and `wiki/hot.md`.
