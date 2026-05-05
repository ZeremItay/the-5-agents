---
tags:
  - architecture
  - scaffold
status: shipped
---

# Project Scaffold

## Overview

`TESTTOMORROW` הוא פרויקט לצוות סוכנים ליצירת תוכן, מנוהל על ידי **סוכן ראשי (מנכ"ל)** + צוות סוכנים מתמחים שיוגדרו בהמשך. בשלב הנוכחי הוקם שלד תשתיתי בלבד: שורש ריפו עם `CLAUDE.md`, קונפיג סביבה (`.env` / `.env.example` / `.gitignore`), ותיקיית `.claude/` עם `settings.json` ושלוש תת-תיקיות (`agents/`, `skills/`, `commands/`). ה-Plugin `superpowers` הותקן ב-project scope, ובתיקיית `.claude/skills/` יושבים שלושה skills של Obsidian. הקוד פוסט ל-[ZeremItay/the-5-agents](https://github.com/ZeremItay/the-5-agents).

## Open Questions

- מי הם הסוכנים בצוות (מנכ"ל + איזה תפקידים נוספים)? עוד לא הוגדר
- איזה stack טכני לכתיבת הסוכנים — Claude Agent SDK ב-Python? Node? משהו אחר?
- האם ייווצרו slash commands ייעודיים לפרויקט ב-`.claude/commands/`, ומתי

## Session Log

### 2026-05-05 — Initial scaffold and vault initialization [shipped]
- **What was done:** הוקמו `CLAUDE.md`, `.env`/`.env.example`, `.gitignore`, `.claude/{agents,skills,commands}/`. הותקן `superpowers` plugin ב-project scope. אותחלה תיקיית `vault/` עם פרוטוקול Obsidian Vault Workflow.
- **Decisions:** project-scoped plugin (לא user) כדי שהפרויקט יהיה עצמאי וכל מי שיעשה clone יקבל אותה הגדרה. `vault/` נכלל ב-git כדי שהזיכרון של הפרויקט יישמר עם משתפי פעולה.
- **Notes / Caveats:** עדיין אין סוכנים מוגדרים — `agents/` ו-`commands/` מחזיקים רק `.gitkeep`. ההחלטות על stack טכני נדחו למפגש הבא.
- **Related:** [[claude-md]], [[env-files]], [[gitignore]], [[claude-code-config]], [[claude-subfolders]], [[obsidian-skills]], [[superpowers-plugin]], [[git-and-github]]
