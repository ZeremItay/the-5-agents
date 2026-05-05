---
tags:
  - config
  - git
status: shipped
---

# .gitignore

## Overview

`.gitignore` קובע אילו קבצים ותיקיות **לא** יוכלו להיכנס ל-git, גם אם מנסים להוסיף אותם בטעות. בפרויקט הזה הוא חוסם בעיקר שלוש קטגוריות: (1) קבצי סביבה עם סודות (`.env`, `.env.local`, `.env.*.local`), (2) פסולת של מערכות הפעלה ועורכים (`.DS_Store`, `Thumbs.db`, `.vscode/`, `.idea/`, `*.swp`), ו-(3) artefacts של stack-ים אפשריים (`node_modules/`, `__pycache__/`, `.venv/`).

**משויך לכלל הפרויקט / שכבת הביטחון של git.**

## Open Questions

- כשייכתב קוד אמיתי — האם נוסיף ignored לקבצי build נוספים (`dist/`, `build/`, `*.log`, IDE specifics)?
- האם להוסיף `.aider/` או artefacts של כלי AI אחרים אם ישולבו בפרויקט?

## קבצים קשורים בפרויקט

- `.gitignore` (שורש)

## Session Log

### 2026-05-05 — Created initial .gitignore [shipped]
- **What was done:** הוקם `.gitignore` עם רשימה ראשונית: env files, OS junk, IDE files, Node modules, Python venvs.
- **Decisions:** הוספנו ignored ל-Python ו-Node מראש למרות ששתיהן עדיין לא בשימוש — כי אחת מהן בוודאי תיכנס בהמשך וזה זול לכלול עכשיו. שמנו את `.env` בשורה השנייה (גלוי וברור) כדי שיהיה קשה למחוק בטעות.
- **Notes / Caveats:** אומת ש-`.env` נחסם נכון לפני קומיט הראשון של ה-`.env.example`.
- **Related:** [[env-files]], [[git-and-github]]
