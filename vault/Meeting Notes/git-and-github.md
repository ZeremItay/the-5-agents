---
tags:
  - git
  - github
  - infrastructure
status: shipped
---

# Git & GitHub Setup

## Overview

הפרויקט הוא ריפו git (אותחל ב-`git init -b main`) שמסונכרן עם GitHub ב-[ZeremItay/the-5-agents](https://github.com/ZeremItay/the-5-agents). ה-remote `origin` מוגדר וה-branch המקומי `main` עוקב אחרי `origin/main`. נכון ל-2026-05-05 קיימים שלושה קומיטים: scaffold ראשוני, env template + gitignore, והתקנת superpowers plugin. הגישה לדחיפה דרך אימות GitHub של חשבון `ZeremItay` (Git Credential Manager של Windows).

**משויך לכלל הפרויקט / שכבת השליטה בגרסאות.**

## Open Questions

- האם להוסיף branch protection / CODEOWNERS / templates של PR ב-GitHub?
- האם נצטרך branches נפרדים (`dev`, `staging`) או מספיק `main` בשלב הזה?
- האם להוסיף GitHub Actions (CI/lint/tests) — ייקבע כשייכתב קוד אמיתי

## קבצים קשורים בפרויקט

- `.gitignore` — מתעד מה לא נכנס ל-git (ראה [[gitignore]])
- `.git/` (לא ב-vault — תיקיית metadata פנימית של git)

## היסטוריית קומיטים

```
7571570  Install superpowers plugin at project scope
5e01f01  Add .env.example template and .gitignore
814302a  Initial project scaffold for TESTTOMORROW agent team
```

## Session Log

### 2026-05-05 — Initial git setup and three commits [shipped]
- **What was done:** `git init -b main` בתיקיית הפרויקט. נוצרו שלושה קומיטים. ה-remote הוגדר ל-`https://github.com/ZeremItay/the-5-agents.git` (אחרי תיקון של URL שגוי בהתחלה — היה `the-5-agents-workshop` בטקסט אבל `the-5-agents` ב-href, בנוסף לבלבול קצר עם `danielleshitrit4-stack/TESTGIT`).
- **Decisions:** branch ברירת מחדל = `main` (לא `master`). דחיפה דרך HTTPS עם GCM (לא SSH). שלוש "יחידות עבודה" נפרדות = שלושה קומיטים נפרדים, לא commit אחד גדול.
- **Notes / Caveats:** הדחיפה הראשונה הופסקה ע"י המשתמש באמצע, אבל הקומיט הספיק להגיע לשרת לפני שהפקודה בוטלה — ולכן `git ls-remote` כבר הראה את ה-hash אחרי החזרה.
- **Related:** [[gitignore]], [[project-scaffold]], [[claude-code-config]]
