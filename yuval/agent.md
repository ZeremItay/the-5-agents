# יובל (Yuval) — Working Folder

This folder is **Yuval's workspace**. It is not the canonical agent definition — it's the working area where Yuval's reference images live and his outputs are saved.

## What lives here

- `agent.md` — this file (a pointer to the canonical definition)
- `skill.md` — pointer to the skill Yuval uses
- `reference/` — drop inspiration images here (PNG/JPG/WEBP). Yuval scans this folder before each generation to extract style.
- `outputs/` — Yuval saves generated images here. Naming: `<YYYY-MM-DD>-<slug>.png` plus a sibling `.txt` with the prompt used.

## Canonical agent definition

The actual agent (loaded by Claude Code) lives at:

```
.claude/agents/yuval.md
```

That's the file with the YAML frontmatter that Claude Code reads to discover Yuval as a sub-agent. Editing **this** file does **not** change Yuval's behavior — change `.claude/agents/yuval.md` instead.

## How to use Yuval

Just ask the CEO (Reuven) for an image. Reuven routes the request to Yuval automatically. Examples:

- "תייצר לי תמונה של חלל עבודה עתידני"
- "Generate a hero image for the landing page"
- "Make an illustration in the same style as the references in `yuval/reference/`"

If you have inspiration images, drop them in `yuval/reference/` first. If you don't, Yuval will work from your prompt alone.
