---
tags:
  - agent
  - content
  - writer
status: shipped
---

# Yael — Content Writer Agent

## Overview

Yael (יעל) הוא הסוכן השני שמוגדר תחת [[reuven-orchestrator|Reuven]] — סוכנת תוכן (LLM-only) שמטפלת בשכתוב, עריכה, ניסוח מחדש, תרגום וסיכום של מאמרי גלם. היא טוענת את `yael/style-guide.md` ואת כל קובצי `yael/reference/`, מושכת מאמר מ-`Content/`, משכתבת אותו בסגנון הפרויקט, שומרת ב-`Output/<name>.md`, ומעתיקה את המקור ל-`Content/Ready/<name>.md`. כשהיא מזהה צורך בתמונה היא משאירה placeholder בפורמט `{{IMAGE_NEEDED: "..."}}` בתוך הטקסט.

**אילוץ ארכיטקטוני קריטי:** ב-Claude Code סאב-אייג'נטים לא יכולים להפעיל סאב-אייג'נטים אחרים — רק [[reuven-orchestrator|Reuven]] יכול. לכן יעל **לא** קוראת ל-[[yuval-agent|יובל]] ישירות. במקום זה, היא משאירה את ה-placeholders, מדווחת עליהם ל-Reuven, ו-Reuven הוא זה שמפעיל את יובל לכל אחד ומחליף את ה-placeholders ב-markdown image references בקובץ הסופי. בנוסף, יעל לא יכולה למחוק קבצים (אין לה Bash) — היא רק מעתיקה ל-Ready, ו-Reuven מבצע את ה-`rm` של המקור מ-`Content/`.

**משויך:** סאב-אייג'נט של [[reuven-orchestrator|Reuven]]. נטען על ידי Claude Code מ-`.claude/agents/yael.md`.

## Open Questions

- **Trigger keyword conflicts**: "ערוך"/"edit" ו-"תוכן"/"content" גנריים. אם יתווסף בעתיד agent של "code editor" או "publishing" — ייתכן ניגוד. נטר את [[CEO-Logs/_index|CEO-Logs]] ונדייק keywords אם תהיה התנגשות.
- **Empty `style-guide.md` fallback**: כרגע יעל פועלת בנייטרלי ומציינת בדיווח. האם זה מספיק כשהמשתמש שוכח לעדכן, או שצריך אזהרה חמורה יותר (למשל refusal עד שהקובץ ימולא)?
- **רובוסטיות ה-rm**: אם Reuven נכשל אחרי שיעל סיימה (לפני ה-`rm Content/<name>.md`) — נשאר כפיל ב-`Content/`. ה-Bash check (האם `Content/Ready/<name>.md` קיים) מקטין סיכון אבל לא אטומי. ניטור ידני בשלב הזה.
- **תמיכה במספר מאמרים בקריאה אחת**: כרגע יעל מטפלת במאמר אחד לפעולה. האם להוסיף מצב batch (`Glob` כל `Content/*.md` ו-loop)? תלוי בנפח השימוש.
- **שמירת מטא-דאטה**: האם להוסיף frontmatter לקבצים ב-`Output/` (תאריך עיבוד, אורך מקור vs. רwrite, גרסה)? כרגע אין — ה-output הוא Markdown נקי.

## קבצים קשורים בפרויקט

- `.claude/agents/yael.md` — הגדרת הסוכנת הקנונית (frontmatter, system prompt, workflow, hard rules)
- `yael/` (שורש) — תיקיית עבודה של יעל (`style-guide.md` + `reference/` + `agent.md` pointer)
- `Content/` — pipeline inbox למאמרי גלם
- `Content/Ready/` — מאמרים שיעל סיימה לטפל בהם
- `Output/` — תוצרי השכתוב
- `.claude/agents/reuven.md` — Reuven שמנתב משימות תוכן ליעל ומפעיל את יובל על ה-placeholders

## Session Log

### 2026-05-05 — Yael scaffolded as agent-2 [shipped]
- **What was done:** הוטמעו (1) `.claude/agents/yael.md` עם frontmatter (`tools: Read, Write, Edit, Glob, Grep` — בלי Bash, בלי Task, בלי web access; `model: sonnet`) ו-body מפורט: Role, Working Directories, Workflow (7 שלבים: load-style → pick-article → rewrite → image-placeholders → save → copy-to-Ready → report), IMAGE_NEEDED format spec עם regex מדויק, Hard Rules. (2) תיקייה `yael/` בשורש עם `style-guide.md` (skeleton עם 6 sections), `reference/.gitkeep`, ו-`agent.md` (pointer doc). (3) שלוש תיקיות pipeline ב-שורש: `Content/`, `Content/Ready/`, `Output/` — כולן עם `.gitkeep`. (4) עדכון [[reuven-orchestrator|Reuven]] להוסיף את יעל כ-agent-2 בטבלת ה-routing, להוסיף סעיף "Post-Yael Cleanup & Image Pipeline" ב-Decision Protocol, ולעדכן Status ל-"2 of 4 — Partially operational".
- **Decisions:** **LLM-only tools בלבד** (Read, Write, Edit, Glob, Grep) — בכוונה. יעל היא סוכנת ניסוח, לא orchestrator. אין לה Bash (אז אי אפשר למחוק קבצים — Reuven עושה את זה), אין לה Task (אז אי אפשר לקרוא ליובל — placeholders במקום), אין לה web/API. **`Content/`, `Content/Ready/`, `Output/` בשורש הפרויקט** ולא תחת `yael/` — אלו pipeline folders משותפים שאגנטים אחרים עתידיים יוכלו להשתמש בהם. **`{{IMAGE_NEEDED: "..."}}` syntax** מובחן וקל ל-grep, לא מתנגש ב-Markdown סטנדרטי. **יעל מעתיקה ל-Ready ולא מוחקת** — Reuven אחראי על ה-`rm` כי יש לו Bash וכי הוא ה-orchestrator. **Yael = agent-2** ב-`.claude/agents/reuven.md` בעקבות yuval (agent-1). **`style-guide.md` נכתב כ-skeleton** עם section headers ובלי תוכן — המשתמש ימלא ידנית. אם הוא נשאר ריק, יעל פועלת בנייטרלי ומציינת בדיווח.
- **Notes / Caveats:** ה-trigger keywords של יעל ("ערוך", "תוכן", "edit", "content") גנריים — עלול להיות ניגוד עם אגנטים עתידיים. נטר את [[CEO-Logs/_index|CEO-Logs]]. ה-pipeline של מחיקת המקור מ-`Content/` תלוי ב-Reuven שמבצע `rm` אחרי שיעל מסיימת — אם Reuven נופל באמצע, ייתכן כפיל ב-`Content/` ו-`Content/Ready/`. ה-image pipeline סדרתי (יובל נקרא לכל placeholder בנפרד) — אם יש 5 placeholders זה 5 קריאות ל-OpenAI API.
- **Related:** [[reuven-orchestrator]], [[yuval-agent]], [[gpt-image-gen-skill]], [[claude-subfolders]], [[claude-md]]
