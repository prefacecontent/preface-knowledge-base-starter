---
name: identity
description: "Build the identity layer that turns this knowledge base into a digital replica. Interviews the person section by section and fills out wiki/context/ (core, identity, thinking, voice, policies, preferences) so the agent can reason and write AS them. Optionally web-searches public info to fill gaps, with permission. Resumable across passes. Triggers on: build my identity, set up the context layer, fill in wiki/context, make this my replica, interview me, /identity, who am I, set up the persona, create my digital twin."
allowed-tools: Read Write Edit Glob Grep WebSearch WebFetch
---

# identity: Build the Identity Layer

The `wiki/` knowledge base holds *what* this brain knows. The identity layer in `wiki/context/` holds *who it is* and *how it thinks*, and it wins on conflict. This skill fills that layer by interviewing the person, one section at a time, then writing their answers into the right context files.

Run it like a conversation, not a form. A few focused questions per section. The goal is a context pack rich enough that the agent can reason and draft AS the person, not merely about them.

Use plain file operations: `Read`/`Grep`/`Glob` to inspect existing context files, `Write`/`Edit` to fill them. `WebSearch`/`WebFetch` only when the person grants permission to look up public information.

---

## Before you start

1. **Say what this is.** One or two lines: "I'm going to build your identity layer so I can reason and write as you. I'll ask a few questions per section and fill in `wiki/context/`. We can do it all now or in passes." 
2. **Check what's already filled.** Read each file in `wiki/context/`. A file still holding `[TEMPLATE]` placeholders or `[ ]` blanks is unfilled. Report the state: "core and identity are done; thinking, voice, policies, preferences are still templates." 
3. **Offer to skip or refine.** For already-filled sections, ask whether to skip them or refine them. Never silently overwrite human-reviewed content; `wiki/context/` is the source of truth.
4. **Offer web research up front.** Ask once: "Shall I look up public information about you or your company to fill gaps as we go? I'll mark anything I find as needing your confirmation." Respect the answer for the whole session.

---

## The interview, section by section

Work through the sections in this order. Each maps to one context file and covers part of the five-part replica checklist (identity, knowledge, work history, leadership and decision-making, communication). Ask the questions conversationally, in the person's words; do not read the list aloud like a survey. After each section, write the file before moving on, so progress survives an interruption.

### 1. core.md: the spine
The irreducible few lines, loaded on every task.
- Who are you in one or two lines: name, role, organisation?
- What shaped you (training, career path) in a sentence?
- The single sentence that captures your decision-making instinct?
- How should the agent sound as you (register and manner)?
- The two or three things the agent must **never get wrong** about you?

### 2. identity.md: background and history
Career, roles, timeline, expertise, major work.
- Department and remit; your core areas of expertise (the hard knowledge you own)?
- Your timeline: the roles and milestones that matter, with rough years?
- Your two or three biggest projects or initiatives, your part and the outcome?
- Bodies of past work or sources someone should know you by?

### 3. thinking.md: how you decide
Principles, mental models, trade-offs, risk, lessons.
- The mental models or principles you actually reason from?
- Your trade-off rules: when X, you prefer A over B, because...?
- Your risk posture: how do you treat downside, uncertainty, irreversible moves?
- Reusable patterns that have worked, and the hard lessons you avoid repeating?
- How do you give feedback and run a decision with others?

### 4. voice.md: how you express yourself
Tone, recurring phrases, drafting do's and don'ts.
- Your tone and register: measured, warm, direct, technocratic...?
- Phrases or vocabulary you genuinely use (only real ones)?
- When you write, what do you lead with, how long are your sentences, how certain do you sound?
- What should the agent avoid when writing as you (hype, jargon, prediction...)?

### 5. policies.md: boundaries
Limits, sensitive topics, never-fabricate rules.
- Topics to handle with care or not at all in your name?
- Things never to claim or fabricate: quotes, figures, credentials?
- Anything confidential that must never leave the vault?
- Any regulated advice that must not be given in your name?

### 6. preferences.md: output choices
Format, language, level of detail by context.
- Default language and any others you work in?
- Format you prefer: bullets vs prose, length, structure?
- How does your level of detail change by audience (board vs working team vs public)?
- How you like things framed, and how to open and close?

---

## Web research (with permission)

If the person agreed, you may search to enrich a section *during* the interview, and do a final gap-filling pass *after*. Use `WebSearch` to find public material (their company site, talks, articles, profiles) and `WebFetch` to read a specific page.

Rules that keep this honest:
- **Ask before each substantive lookup** if it was not already cleared, and tell them what you are about to look up.
- **Mark every web-gathered fact as unconfirmed** in the file, e.g. `> [!unverified] From [source], please confirm.` so the person can vet it.
- **Never fabricate quotes.** If you cannot find a real phrase someone uses, leave voice low-fidelity rather than inventing one. Restraint beats a convincing fake.
- Public bios are often wrong or stale. Treat them as prompts for a question, not as truth.

---

## Writing the files

- **Replace the placeholders.** Swap `[NAME]`, `[TEMPLATE]`, and `[ ]` blanks for the person's real answers. Do not leave template scaffolding behind in a section you have filled.
- **Keep the YAML frontmatter shape.** Preserve each file's existing frontmatter keys. Update `last_reviewed` to today and set `confidence` to `human-reviewed` once the person has confirmed a section (leave it `template` until then).
- **House style: English (UK), no em or en dashes.** Match the trimmed, declarative tone of the other context files.
- **In INDEX.md**, leave the routing table as is. You may set the persona's name where the file references it; do not change the load rules.
- Write the knowledge, not the conversation: "Reasons from unit economics first," not "They told me they like unit economics."

---

## Finishing and resuming

- After the last section, **let the person review.** Summarise what each file now says in a line or two and invite corrections. Apply any edits.
- Remind them: `wiki/context/` is the **human-reviewed source of truth** and overrides the `wiki/` knowledge base when they disagree. It is worth getting right.
- This is **resumable**. If the session ends early, the files written so far stand on their own. On a later run, re-detect filled sections and offer to skip or refine, so the person can build their replica in passes.
