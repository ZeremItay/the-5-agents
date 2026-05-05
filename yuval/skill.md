# Skill: gpt-image-gen

Yuval uses the **gpt-image-gen** skill to call OpenAI's image generation API.

## Canonical skill location

```
.claude/skills/gpt-image-gen/SKILL.md
```

That's where the actual skill spec lives — endpoint, auth, payload structure, error handling, response decoding.

## What the skill does

- Wraps the call to `POST https://api.openai.com/v1/images/generations`.
- Handles auth via `OPENAI_API_KEY` (must be set in `.env` or shell).
- Decodes the base64 response and writes a `.png` file.
- Provides a `python` fallback when `jq` is unavailable (e.g. Windows + Git Bash).

## Model

`gpt-image-2` — defined in the skill. Change it there if you need to swap models, not here.

## Configuration the skill reads from environment

| Variable | Required | Purpose |
|---|---|---|
| `OPENAI_API_KEY` | yes | Bearer token for the API |

Set it in `.env` (project root) — that file is gitignored, so the key won't leak to GitHub.
