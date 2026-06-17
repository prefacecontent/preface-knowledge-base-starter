---
description: Save the current conversation or a specific insight into the wiki vault as a structured note.
---

Read the `save` skill. Then run the save workflow for this conversation.

Usage:
- `/save` — analyze the full conversation and save the most valuable content
- `/save [name]` — save with a specific note title (skip the naming question)
- `/save session` — save a complete session summary into `wiki/sessions/`
- `/save concept [name]` — explicitly save as a concept page
- `/save synthesis [name]` — explicitly save as a synthesis page

Check if a page with the same name already exists. If it does, offer to update it instead of creating a duplicate.
