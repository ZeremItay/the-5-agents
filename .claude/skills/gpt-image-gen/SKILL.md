---
name: gpt-image-gen
description: Generate images via OpenAI's image generation API (gpt-image-2). Use when a task requires creating, generating, designing, or producing an image from a text prompt. The skill encapsulates the full API call — endpoint, auth, payload, response decoding — and saves the resulting PNG to a path the caller specifies.
---

# GPT Image Generation Skill

Generate a single image from a text prompt using OpenAI's `/v1/images/generations` endpoint.

## When to invoke

Any time a user task or upstream agent needs an image generated from text. Typical triggers:
- "generate an image of...", "create a picture of...", "produce a hero image..."
- "תמונה של...", "ציור של...", "להפיק תמונה", "תייצר תמונה"
- A request that produces a visual asset (landing page hero, blog cover, illustration, mockup)

## Prerequisites

- `OPENAI_API_KEY` environment variable must be set (read from `.env` or shell). If missing, abort with a clear error — do NOT fall back to a placeholder or skip the call.
- `curl` is required.
- For decoding the base64 response: `jq` if available, otherwise `python` (the fallback below works on Windows Git Bash, macOS, and Linux).

## API call template (with jq)

```bash
curl -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-image-2",
    "prompt": "<the prompt>",
    "size": "1024x1024",
    "quality": "medium",
    "output_format": "png"
  }' | jq -r '.data[0].b64_json' | base64 --decode > <output-path>.png
```

## Python fallback (when jq is unavailable)

```bash
RESP=$(curl -sS -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"gpt-image-2","prompt":"<the prompt>","size":"1024x1024","quality":"medium","output_format":"png"}')

echo "$RESP" | python -c "import sys,json,base64; d=json.load(sys.stdin); open('<output-path>.png','wb').write(base64.b64decode(d['data'][0]['b64_json']))"
```

## Parameters

| Field | Values | Default | Notes |
|---|---|---|---|
| `model` | `gpt-image-2` | `gpt-image-2` | Locked to this model unless caller overrides. |
| `prompt` | natural language | (required) | Keep under ~1000 chars; specific > verbose. |
| `size` | `1024x1024`, `1024x1536`, `1536x1024` | `1024x1024` | Square / portrait / landscape. |
| `quality` | `low`, `medium`, `high` | `medium` | Higher = slower + more expensive. |
| `output_format` | `png`, `jpeg`, `webp` | `png` | PNG supports transparency. |

## Output handling

1. The API returns JSON with `.data[0].b64_json` containing the base64-encoded image.
2. Decode and write to a deterministic path. **The caller picks the path** — this skill does not invent one.
3. After writing, **verify file size > 0 bytes** (`wc -c < <path>` or `ls -la <path>`). A 0-byte file means the API returned an error response — re-read the JSON for the `error.message` field.

## Error handling

- **401 Unauthorized** — `OPENAI_API_KEY` is missing or invalid. Stop, do not retry.
- **400 Bad Request** — usually a content-policy violation or invalid parameter. Read `error.message` in the response body.
- **429 Rate limit** — retry with exponential backoff (start 5s, max 60s, max 3 retries).
- **5xx** — transient; retry once after 10s.

## Response shape

```json
{
  "created": 1735689600,
  "data": [
    {
      "b64_json": "<base64-encoded image>",
      "revised_prompt": "<the prompt OpenAI actually used; may differ from input>"
    }
  ]
}
```

`revised_prompt` is useful for logging — it shows what the model actually rendered, which sometimes differs from the input prompt due to safety rewrites.

## What this skill does NOT do

- ❌ Decide what to generate — that's the caller's job (e.g. the [yuval](../../agents/yuval.md) agent).
- ❌ Analyze reference images for style — also the caller's job.
- ❌ Manage versioning, naming conventions, or directory structure for outputs — caller picks the path.
- ❌ Retry automatically on rate limits — caller decides retry policy.

## Reference

OpenAI Images API: https://platform.openai.com/docs/api-reference/images
