# חן (Chen) — Working Folder

This folder is **Chen's workspace**. It is not the canonical agent definition — it's the working area where Chen's search memory lives.

## What lives here

- `agent.md` — this file (a pointer to the canonical definition).
- `Memory/searches.md` — Chen's search log. Every search she runs gets an entry: keywords, queries tried, sources found, source chosen, file saved. She reads this **before** every new search to avoid duplicates and respect the 30-day cache window.

## Canonical agent definition

The actual agent (loaded by Claude Code) lives at:

```
.claude/agents/chen.md
```

That's the file with the YAML frontmatter that Claude Code reads to discover Chen as a sub-agent. Editing **this** file does **not** change Chen's behavior — change `.claude/agents/chen.md` instead.

## The research → rewrite pipeline

Chen drops fetched articles into the project-root pipeline that Yael also operates on:

- `Content/<YYYY-MM-DD>-<slug>.md` — Chen writes here (frontmatter with `source_url`, `source_title`, etc., plus the article body).
- `Content/Ready/` — Yael copies the source here after she rewrites it.
- `Output/` — Yael's rewrites (with `{{IMAGE_NEEDED}}` placeholders that Reuven replaces with Yuval's images).

Chen does not touch `Content/Ready/` or `Output/`. She only writes to `Content/<YYYY-MM-DD>-<slug>.md` and to `chen/Memory/searches.md`.

## How to use Chen

Just ask the CEO (Reuven). Examples:

- "מצא לי מאמר על השקת Claude 4.7"
- "תחפש מה Anthropic פרסמו השבוע על agent harnesses"
- "חפש 3 מאמרים מקיפים על RAG ב-2026"
- "Find the latest news on OpenAI's agent SDK"

Reuven routes the request to Chen. Chen searches, fetches, filters, saves. If the original request also implied a rewrite ("תכתוב לי מאמר על...", "rewrite the latest news..."), Reuven auto-chains to Yael after Chen finishes. If it was just "find" / "search" / "research", Reuven stops at Chen and returns her file + source link to you.

## Architectural note

Sub-agents in Claude Code cannot invoke other sub-agents — only Reuven can. That's why Chen never calls Yael directly: she just drops the file in `Content/` and tells Reuven what she found. Reuven decides whether to auto-chain.

## Memory growth

`chen/Memory/searches.md` accumulates entries indefinitely (no auto-archival). For now, leave it growing — when it gets unwieldy (5000+ lines), we'll add a date-based archive scheme.
