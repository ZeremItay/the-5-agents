---
tags:
  - skill
  - api
  - image-generation
  - openai
status: shipped
---

# Skill: gpt-image-gen

## Overview

Skill מקומי תחת `.claude/skills/gpt-image-gen/SKILL.md` שמאפשר יצירת תמונות דרך OpenAI's `/v1/images/generations` API. הוא מסכם את כל ה-mechanics של הקריאה — endpoint, auth (`OPENAI_API_KEY`), payload (model `gpt-image-2`, prompt, size, quality, output_format), decode של b64_json, וכתיבה לקובץ PNG. כולל גם python fallback כש-jq לא זמין (רלוונטי ל-Windows Git Bash שבו ראינו ש-jq חסר). משמש כרגע על ידי [[yuval-agent|יובל]], אבל זמין לכל סוכן עתידי שמייצר תמונות.

**משויך:** [[yuval-agent|Yuval]] (כרגע הצרכן היחיד). זמין לכל סוכן ש-`description` שלו תואם.

## Open Questions

- האם המודל `gpt-image-2` באמת קיים ב-API של OpenAI ב-2026-05? `gpt-image-1` היה הקיים נכון לידע הקודם. אם הקריאה תחזיר 404/model not found — נצטרך לעדכן
- האם להוסיף תמיכה ב-`n` (multi-variant generation בקריאה אחת)? OpenAI תומך, ה-skill כרגע מניח 1
- האם להוסיף תמיכה ב-image edit (image-to-image) או רק text-to-image? כרגע רק text
- האם להוסיף retry logic ב-skill עצמו (429 + 5xx) או להשאיר ל-caller?
- מה הסכום שאנחנו מצפים להוציא חודשית? אין כרגע בקרת עלויות

## קבצים קשורים בפרויקט

- `.claude/skills/gpt-image-gen/SKILL.md` — קובץ ה-skill הקנוני
- `.claude/agents/yuval.md` — הצרכן העיקרי כרגע
- `.env.example` / `.env` — מקור ה-`OPENAI_API_KEY`
- `yuval/skill.md` — pointer doc בתיקיית Yuval

## Session Log

### 2026-05-05 — Skill created for OpenAI image API [shipped]
- **What was done:** נוצר `.claude/skills/gpt-image-gen/SKILL.md` עם frontmatter מלא (`name: gpt-image-gen`, `description` שמטריג על "generate image", "תמונה של" וכו'). ה-body כולל: prerequisites (`OPENAI_API_KEY` חובה, `curl` + `jq` או `python`), API call template (curl + jq pipeline בדיוק כמו שהמשתמש סיפק), python fallback ל-decode (כי jq לא זמין ב-Git Bash הזה), טבלת parameters (model/prompt/size/quality/output_format), error handling (401/400/429/5xx), response shape, ו-"What this skill does NOT do" כדי לא לבלבל את ה-caller.
- **Decisions:** **המודל `gpt-image-2`** נשמר כפי שהמשתמש כתב — גם אם הוא לא קיים בפועל, הקריאה תחזיר שגיאה ברורה והמשתמש יוכל לעדכן. **Python fallback** נכלל כי בסשן הזה ראינו ש-jq לא מותקן ב-Git Bash, ולא רוצים שהסוכן ייתקע כש-jq חסר. **Skill לא בוחר את path הפלט** — ה-caller (Yuval) מחליט. מאפשר שימוש חוזר בסקיל גם לסוכנים אחרים בעתיד עם conventions שונים. **No retry built in** — השארנו ל-caller כי policy של retry תלוי בסיטואציה.
- **Notes / Caveats:** ה-skill נטען רק אחרי restart של Claude Code (כמו כל skill חדש). יוצרים אותו project-scoped (תחת `.claude/skills/`) ולא user-scoped, כדי שיגיע עם clone של הפרויקט. אם המודל `gpt-image-2` לא קיים — נעדכן ל-`gpt-image-1` או דגם נוכחי אחר; שינוי קל ב-template של ה-API call.
- **Related:** [[yuval-agent]], [[claude-subfolders]], [[obsidian-skills]], [[env-files]]
