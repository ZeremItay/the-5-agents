# יעל (Yael) — Working Folder

This folder is **Yael's workspace**. It is not the canonical agent definition — it's the working area where Yael's style guide and reference texts live.

## What lives here

- `agent.md` — this file (a pointer to the canonical definition).
- `style-guide.md` — the project's writing style guide. **Populate this manually** with voice, tone, vocabulary, and structural patterns. Yael reads it at the start of every session.
- `reference/` — drop reference texts here (any `.md` or `.txt` files) that exemplify the style you want. Yael scans this folder before each rewrite to internalize voice.

## Canonical agent definition

The actual agent (loaded by Claude Code) lives at:

```
.claude/agents/yael.md
```

That's the file with the YAML frontmatter that Claude Code reads to discover Yael as a sub-agent. Editing **this** file does **not** change Yael's behavior — change `.claude/agents/yael.md` instead.

## The content pipeline

Yael operates on a three-folder pipeline that lives at the project root (not under `yael/`):

- `Content/` — drop raw articles here for Yael to process.
- `Content/Ready/` — Yael copies the source here after she finishes (Reuven removes the original from `Content/` afterwards).
- `Output/` — Yael's rewrites. Same filename as the input.

## How to use Yael

Just ask the CEO (Reuven) to rewrite, edit, translate, or summarize content. Reuven routes the request to Yael automatically. Examples:

- "תשכתב לי את המאמר ב-Content/2026-05-05-bull-rider.md"
- "Rewrite the article in Content/ in our style"
- "סכם לי את הטקסט ב-Content/draft.md"

Yael loads `style-guide.md` + `reference/` first, then rewrites the article. If the rewrite calls for images, she leaves `{{IMAGE_NEEDED: "..."}}` placeholders. Reuven sees the placeholders, dispatches each one to Yuval, and replaces them with image links in the final output.

## Architectural note

Sub-agents in Claude Code cannot invoke other sub-agents — only Reuven can. That's why Yael writes `{{IMAGE_NEEDED}}` placeholders instead of calling Yuval directly: Reuven is the orchestrator that ties the two together.
