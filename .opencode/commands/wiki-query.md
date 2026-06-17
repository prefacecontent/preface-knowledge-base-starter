---
description: Ask a question against the wiki — reads hot cache, index, and relevant pages, synthesizes an answer with citations, and files good answers back.
---

Read the `wiki-query` skill. Then run the query workflow:

Usage:
- `/wiki-query <question>` — standard mode (hot → index → drill).
- `/wiki-query quick <question>` — hot cache only, fast.
- `/wiki-query deep <question>` — read the full wiki and optionally supplement with web search.

If the answer is valuable, offer to file it back into `wiki/synthesis/`.
