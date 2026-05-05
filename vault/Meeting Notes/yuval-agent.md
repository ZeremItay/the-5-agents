---
tags:
  - agent
  - creative
  - image-generation
status: shipped
---

# Yuval — Creative Image Generation Agent

## Overview

Yuval (יובל) הוא הסוכן הראשון שמוגדר תחת [[reuven-orchestrator|Reuven]] — סוכן קריאייטיב שמטפל ביצירת תמונות. כל בקשה ויזואלית שמגיעה ל-Reuven מנותבת ל-Yuval. Yuval סורק תמונות השראה ב-`yuval/reference/`, מזהה סגנון/פלטה/קומפוזיציה, מנסח prompt שמשלב את הבקשה עם הסגנון, מייצר תמונה דרך [[gpt-image-gen-skill|gpt-image-gen]] (קריאת API ל-OpenAI), ושומר ב-`yuval/outputs/`. המטרה: עקביות ויזואלית בין כל התמונות שנוצרות בפרויקט.

**משויך:** סאב-אייג'נט של [[reuven-orchestrator|Reuven]]. נטען על ידי Claude Code מ-`.claude/agents/yuval.md`.

## Open Questions

- האם להוסיף שלב "human review" אחרי הג'נרציה הראשונה לפני שהתמונה נחשבת "סופית"? כרגע: שמירה ישירה ב-outputs/, המשתמש מחליט אם לקבל
- האם Yuval צריך לתעד את ה-`revised_prompt` של OpenAI ב-log? כרגע מומלץ אבל לא חובה
- מה קורה כשיש הרבה reference images מתחומים שונים — Yuval בוחר את הרלוונטיים, אבל איך לקבוע "רלוונטיות" אמיתית? כרגע לפי domain matching בלבד
- האם לתמוך בגרסאות מרובות (variants) בקריאה אחת? OpenAI API תומך ב-`n` parameter, אבל הסקיל כרגע מוגדר ל-1
- האם להוסיף יכולת image edit (image-to-image) או image variation? כרגע רק generation מ-text

## קבצים קשורים בפרויקט

- `.claude/agents/yuval.md` — הגדרת הסוכן הקנונית (frontmatter, system prompt, workflow)
- `.claude/skills/gpt-image-gen/SKILL.md` — הסקיל שYuval מפעיל
- `yuval/` (שורש) — תיקיית עבודה של Yuval (`reference/` + `outputs/` + docs)
- `.env.example` / `.env` — `OPENAI_API_KEY` נדרש לפעולה

## Session Log

### 2026-05-05 — Yuval scaffolded as first sub-agent [shipped]
- **What was done:** הוטמעו (1) `.claude/agents/yuval.md` עם frontmatter (`tools: Read,Write,Edit,Bash,Glob,Grep`, `model: sonnet`) ו-body מפורט: Role, Workflow (7 שלבים: scan → extract → prompt → size/quality → generate → verify → report), Hard Rules, naming convention. (2) תיקייה `yuval/` בשורש עם `reference/.gitkeep`, `outputs/.gitkeep`, ו-2 קבצי docs (`agent.md`, `skill.md` כ-pointer files). (3) [[gpt-image-gen-skill]] חדש (ראה topic נפרד). (4) הוספת `OPENAI_API_KEY` ל-`.env` ו-`.env.example`. (5) עדכון [[reuven-orchestrator|Reuven]] להחזיק את Yuval כ-agent-1 בטבלת ה-routing — Reuven כעת חצי-מבצעי (1/4 סוכנים).
- **Decisions:** **מבנה היברידי** — קובץ סוכן ב-`.claude/agents/yuval.md` (כי Claude Code דורש flat file לגילוי) + תיקייה `yuval/` בשורש לתוכן (reference + outputs). הקבצים `yuval/agent.md` ו-`yuval/skill.md` הם pointer docs לבני אדם, לא הגדרות ל-Claude Code. **תיקיית `reference/` ביחיד** (לא `references/`) לעקביות עם curl conventions של ה-API. **שמירת outputs** עם naming `YYYY-MM-DD-slug.png` + sibling `.txt` עם ה-prompt לאיטרציה. **Yuval ממוקם כ-agent-1** ב-reuven.md כי הוא הראשון שמוגדר; היתר עדיין TBD.
- **Notes / Caveats:** ה-`OPENAI_API_KEY` חייב להיות מוגדר ב-`.env` המקומי לפני קריאה — אם חסר, Yuval חייב לעצור ולא להמשיך עם dummy call. ה-skill כולל python fallback ל-decode base64 כי `jq` לא בהכרח מותקן ב-Git Bash על Windows. המודל `gpt-image-2` נשאר כפי שהמשתמש כתב — אם המודל לא קיים בפועל, הקריאה תחזיר שגיאה ברורה ונעדכן ל-`gpt-image-1` או דגם מקביל.
- **Related:** [[reuven-orchestrator]], [[gpt-image-gen-skill]], [[claude-subfolders]], [[env-files]], [[claude-md]]
