---
tags:
  - config
  - environment
  - secrets
status: shipped
---

# Environment Files (`.env`, `.env.example`)

## Overview

שני קבצים שמרכזים משתני סביבה לפרויקט: `.env.example` הוא **תבנית** שעולה ל-git ומראה איזה משתנים נדרשים (בלי ערכים אמיתיים), ו-`.env` הוא הקובץ **המקומי** עם הערכים האמיתיים — הוא חסום ב-[[gitignore]] ואסור שיעלה לריפו. כיום מוגדרים בהם שלושה משתנים: `ANTHROPIC_API_KEY`, `ANTHROPIC_MODEL`, `LOG_LEVEL`.

**משויך לכל קוד שיריץ סוכנים מול ה-API של Anthropic / Claude.**

## Open Questions

- האם נצטרך מפתחות API נוספים בעתיד (Google? Slack? Make?) — תלוי בסוכנים שייכתבו
- האם להוסיף `.env.production` / `.env.development` בנפרד? עדיין לא רלוונטי
- מה ערך ברירת המחדל ל-`ANTHROPIC_MODEL` (אם המשתמש משאיר אותו ריק)?

## קבצים קשורים בפרויקט

- `.env.example` (שורש) — תבנית, נכלל ב-git
- `.env` (שורש) — מקומי, ב-`.gitignore`

## Session Log

### 2026-05-05 — Created env template and local env [shipped]
- **What was done:** נוצרו `.env.example` ו-`.env` עם שלושה משתנים: `ANTHROPIC_API_KEY`, `ANTHROPIC_MODEL`, `LOG_LEVEL`. נוסף `.gitignore` שחוסם את `.env` ואת וריאציות `.env.local`/`.env.*.local`.
- **Decisions:** `.env.example` ו-`.env` מתחילים זהים בתוכן (תבנית); המשתמש ממלא את הערכים האמיתיים ב-`.env` ידנית. בחרנו לא להגדיר ערכי default ב-`.env.example` כדי לא ליצור confusion.
- **Notes / Caveats:** `.env` בפועל ריק כרגע (ללא API key). המשתמש צריך להכניס את המפתח שלו מ-https://console.anthropic.com/settings/keys.
- **Related:** [[gitignore]], [[project-scaffold]]
