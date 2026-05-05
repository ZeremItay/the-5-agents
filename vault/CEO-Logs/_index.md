---
tags:
  - vault
  - index
  - ceo-logs
---

# CEO Logs — Index

לוגים יומיים של החלטות הניתוב של [[reuven-orchestrator|Reuven]] — סוכן המנכ"ל. כל יום קובץ נפרד `YYYY-MM-DD.md`. נכתבים אוטומטית ע"י Reuven ב-runtime.

## למה זה כאן

Reuven מנתב כל משימה לסאב-אייג'נט (אחד מתוך 4) ומסכם את התוצאה למשתמש. ה-logs האלה הם הזיכרון של "מי, מתי, ולמה" — מי קיבל משימה, מתי, מה השיקול לניתוב, ומה התוצאה. שימושי גם לדיבוג, לאיתור דפוסי `out-of-scope`, ולסיכום שבועי/חודשי של הפעילות.

## פורמט entry

````markdown
## [HH:MM] Task #<id>

**משימה:** <תיאור>

**ניתוב:** `<agent-name>`
**שיקול:** <למה נבחר>

**Task tool call:**
- description: <...>
- prompt summary: <...>

**תוצאה:** <סיכום>
**סטטוס:** ✅ הושלם | ⚠️ חלקי | ❌ נכשל

**זמן ריצה:** <X>

---
````

עבור משימה שלא מתאימה לאף סוכן — הכותרת היא `## [HH:MM] Out-of-Scope: <topic>` במקום `Task #<id>`.

הגדרת הפורמט הסמכותית: [[reuven-orchestrator]] (ועוד יותר מפורטת ב-`.claude/agents/reuven.md`).

## נושאים קשורים

- [[reuven-orchestrator]] — הסוכן שכותב את הלוגים האלה
- [[claude-md]] — מטעין את Reuven בתחילת כל סשן
- [[claude-subfolders]] — מבנה הסאב-אייג'נטים שRuven מנתב אליהם
