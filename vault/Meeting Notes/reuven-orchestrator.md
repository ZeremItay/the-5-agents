---
tags:
  - agent
  - orchestrator
  - architecture
status: shipped
---

# Reuven — CEO Orchestrator

## Overview

Reuven הוא סוכן המנכ"ל (CEO orchestrator) של מערכת `TESTTOMORROW`. הוא נקודת הכניסה היחידה לכל משימה: מקבל את הבקשה, בוחר סאב-אייג'נט אחד מתוך 4 על בסיס domain + trigger keywords, מפעיל אותו דרך Task tool של Claude Code, מתעד את ההחלטה והתוצאה ב-[[CEO-Logs/_index|vault/CEO-Logs/]], ומחזיר למשתמש סיכום מסונתז. נכון ל-2026-05-05 הוא **חצי מבצעי** — סאב-אייג'נט אחד ([[yuval-agent|יובל]] — קריאייטיב/תמונות) מוגדר, שלושה נוספים עדיין TBD. בקשות ויזואליות מנותבות ל-Yuval; היתר חוזר כ-Out-of-Scope ומתועד.

**משויך:** Claude Code session-level — כל סשן בפרויקט עוקב אחר הוראות [[claude-md|CLAUDE.md]] שמטעינות את Reuven כ-default entry point.

## Open Questions

- **3 הסאב-אייג'נטים הנותרים** — שמות, תחומי אחריות, trigger keywords, יכולות. Open until next planning session
- **טבלת ה-Routing הסופית** — תושלם כשיוגדרו 3 הנותרים. כרגע yuval ממופה כ-agent-1
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

### 2026-05-05 — Roster updated: yuval added as agent-1 [shipped]
- **What was done:** סאב-אייג'נט ראשון התווסף — [[yuval-agent|יובל]], סוכן קריאייטיב ליצירת תמונות. עודכן `.claude/agents/reuven.md`: (1) ב-"Sub-Agents Under Your Command" שונה `<agent-1>` ל-yuval מלא עם domain + trigger keywords (תמונה של, ציור של, generate image וכו'); (2) ה-callout שונה מ-"Status: TBD" ל-"Partial roster — 1 of 4"; (3) סעיף "Status" בסוף עודכן ל-"🟡 Partially operational" עם הערה על תאריך ה-roster update.
- **Decisions:** **Yuval = agent-1** (לא agent-2/3/4) כי הוא הסוכן הראשון שמוגדר וזה המיקום הטבעי. שמרנו את 3 הסלוטים הנותרים TBD ולא מחקנו את ה-Out-of-Scope flow כי הוא עדיין רלוונטי לרוב המשימות. ה-trigger keywords בעברית ובאנגלית כדי שReuven ינתב גם מבקשות עבריות ('תמונה של...') וגם אנגליות ('generate an image').
- **Notes / Caveats:** Yuval הוגדר במקביל לסקיל [[gpt-image-gen-skill]] שהוא משתמש בו. ה-`OPENAI_API_KEY` נוסף ל-`.env.example` ול-`.env` — חייב להיות מוגדר ידנית לפני שYuval יתחיל לפעול. ה-trigger keywords עשויים להזדקק לליטוש אחרי כמה ניתובים בפועל — נראה דפוסים ב-CEO-Logs.
- **Related:** [[yuval-agent]], [[gpt-image-gen-skill]], [[claude-subfolders]], [[env-files]]
