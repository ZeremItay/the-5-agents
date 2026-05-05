---
tags:
  - agent
  - orchestrator
  - architecture
status: shipped
---

# Reuven — CEO Orchestrator

## Overview

Reuven הוא סוכן המנכ"ל (CEO orchestrator) של מערכת `TESTTOMORROW`. הוא נקודת הכניסה היחידה לכל משימה: מקבל את הבקשה, בוחר סאב-אייג'נט אחד מתוך 4 (TBD) על בסיס domain + trigger keywords, מפעיל אותו דרך Task tool של Claude Code, מתעד את ההחלטה והתוצאה ב-[[CEO-Logs/_index|vault/CEO-Logs/]], ומחזיר למשתמש סיכום מסונתז. נכון ל-2026-05-05 הוא בסטטוס **scaffolded but not operational** — 4 הסאב-אייג'נטים עדיין לא מוגדרים, אז כל משימה מסתיימת ב-`Out-of-Scope` ותתועד ככזאת.

**משויך:** Claude Code session-level — כל סשן בפרויקט עוקב אחר הוראות [[claude-md|CLAUDE.md]] שמטעינות את Reuven כ-default entry point.

## Open Questions

- **4 הסאב-אייג'נטים** — שמות, תחומי אחריות, trigger keywords, יכולות. Open until next planning session
- **טבלת ה-Routing הסופית** — תמולא ב-`.claude/agents/reuven.md` בסעיף "Sub-Agents Under Your Command" כשהסוכנים יוגדרו
- האם יש צורך ב-CLAUDE.md ספציפי לכל סאב-אייג'נט בעתיד?
- **Retry logic** מתקדם יותר (כרגע: max 1 retry per agent) — תלוי בנתוני שימוש בפועל
- **Parallel execution** לסוכנים שלא חופפים — מסומן עתידי ב-PRD סעיף 8
- **Task decomposition** (Reuven שובר משימה מורכבת לתת-משימות) — מסומן עתידי ב-PRD סעיף 8

## קבצים קשורים בפרויקט

- `CLAUDE.md` (שורש) — מטעין את Reuven כ-CEO ב-session start (ראה [[claude-md]])
- `.claude/agents/reuven.md` — הגדרת הסוכן המלאה (frontmatter, system prompt, hard rules, logging format)
- `vault/CEO-Logs/` — תיקיית הלוגים שבה Reuven כותב entries ב-runtime (ראה [[CEO-Logs/_index|CEO Logs]])

## Session Log

### 2026-05-05 — Reuven scaffolded per PRD v1.0 [shipped]
- **What was done:** הוטמעו 6 קבצים לפי PRD Reuven v1.0: (1) `merge` של [[claude-md|CLAUDE.md]] עם בלוק "CEO Orchestrator — Activation Protocol" באנגלית בראש (התיאור הקיים בעברית נשמר). (2) קובץ סוכן חדש `.claude/agents/reuven.md` עם YAML frontmatter (`name: reuven`, `tools: Task,Read,Write,Edit,Glob,Grep`, `model: sonnet`) + body מלא לפי PRD סעיף 6 (Role, Decision Protocol, Hard Rules, Logging Format, Out-of-Scope, Failure Handling, Status). (3) `vault/CEO-Logs/_index.md` חדש — folder index שמסביר את הפורמט. (4) עדכון `vault/_index.md` להוסיף את התיקייה החדשה. (5) topic file הזה. (6) עדכון `vault/Meeting Notes/_index.md` להוסיף את הנושא הזה.
- **Decisions:** **CEO-Logs ב-vault/ בריפו** (לא חיצוני) — נשמר עם הפרויקט, עקבי עם שאר ה-vault, ועובר עם clone. **CLAUDE.md merge ולא replace** — שומר על התיאור העברי של הפרויקט שהיה שם, מוסיף בלוק activation בראש שדומיננטי ל-Claude. **Placeholders + TODO ברורים בקובץ הסוכן** (לא defer) — הסקאפולד מוכן להתחבר לסוכנים ברגע שיוגדרו, בלי drift עם ה-PRD; ה-`> ⚠️ Status: TBD` callout בולט מספיק כדי שלא יישכח.
- **Notes / Caveats:** כל המשימות יחזרו כ-`Out-of-Scope` עד שיוגדרו 4 הסאב-אייג'נטים — זה לא bug, זה ה-state המתוכנן של PRD v1.0. ה-`description` ב-frontmatter תורגם ל"single entry point for the multi-agent system" כדי לעודד את Claude Code לבחור ב-Reuven default על משימות שמגיעות. בחרנו `model: sonnet` (לא opus) כי PRD מציין שזה מספיק לניתוב — sonnet 4.6 הוא הברירת מחדל היעילה כאן.
- **Related:** [[claude-md]], [[claude-subfolders]], [[claude-code-config]], [[project-scaffold]], [[CEO-Logs/_index|CEO Logs]]
