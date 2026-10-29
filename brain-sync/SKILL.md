---
name: brain-sync
description: Use when the user asks to update their second brain from a chat, to sync new learnings into the brain repo, to process brain Inbox, or after a meaningful conversation that should become lasting notes. Also use for /brain sync or "این چت رو به brain ببر".
---

# Brain sync

Goal: turn new facts from a conversation into distilled notes in the user's second-brain repo, without dumping raw chat. Write nothing permanent until the user approves. Commit only when they ask.

## Defaults (override if the user names others)
- Repo: `mohamadreza-rezaei/brain` (private), default branch `main`
- Language: Persian for note body; brand/tech names stay Latin
- Policy files to read first: `BRAIN.md`, then `AGENTS.md`, then `INDEX.md`, then update-status note (`وضعیت-به‌روزرسانی`)
- Inbox path: `Inbox/` (or whatever the repo uses)
- Quality bar: distilled knowledge + meaningful wikilinks; never raw chat archives

## Standard delta format (any tool can emit this)

```yaml
source: cursor | chatgpt | claude | telegram | slack | looply | other
session_date: YYYY-MM-DD
confidence: confirmed | assumed
items:
  - type: fact | decision | task | project_update | preference | learning
    summary: one sentence
    detail: optional short paragraph
    suggest_path: optional path under the vault
    tags: []
```

If another tool only has free text, convert it into this shape before proposing writes.

## Workflow

### 1. Capture
- Prefer a user-supplied delta. Otherwise extract from the current chat (and memory if needed).
- Keep only what is **new and lasting**. Skip chit-chat, one-off commands, and secrets.
- Label each item `confirmed` vs `assumed`. Ask one short question if a material item is unclear.

### 2. Read the brain
- Load `BRAIN.md` / `AGENTS.md` / `INDEX.md` and the relevant existing notes (search before creating).
- Prefer updating an existing note over creating a duplicate.

### 3. Propose (no writes yet)
For each item show:
- action: create | update | skip
- path
- short before → after (or new note outline)
- frontmatter fields if creating

Group by folder. End with a single approval ask: all / pick some / edit / cancel.

### 4. Apply only what was approved
- Create or edit files via the GitHub connector (or local clone if the user has one open).
- Important notes need: full frontmatter (`title`, `type`, `status`, `tags`, `updated`), opening paragraph, current status, decisions/boundaries when relevant, ≥2 wikilinks, callouts for warnings/assumptions.
- Split big topics (Overview / Architecture / Decisions / Status). No giant catch-all files.
- Never store secrets, tokens, or sensitive personal data unless the user explicitly asks.
- Do **not** overwrite old decisions; add a new decision note instead.
- When ingesting from Inbox: read → classify → update/create → link → archive the source file.

### 5. Verify and report
- Re-read changed files.
- Report paths and a 3–6 line Persian summary of what changed.
- **Commit only if the user asked to commit.** Suggested message: `brain: sync from <source> (<date>)`.

## Rules
- Product before tools: skip trendy noise with no lasting value.
- No regenerating the whole brain; patch only what's new.
- No files named `notes.md` / `misc.md` / `chat-1.md`.
- If nothing durable was learned, say so and stop.

## Quick test recipe
1. User pastes a short delta or says "این چت رو به brain ببر".
2. Agent proposes diffs only.
3. User replies "اعمال کن" (optionally "و کامیت کن").
4. Agent writes approved files, verifies, reports links.
