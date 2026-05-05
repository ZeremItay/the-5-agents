---
tags:
  - vault
  - index
  - meeting-notes
---

# Meeting Notes — Index

ארכיטקטורה, החלטות, sessions של קוד ותשתית. נושא אחד = קובץ אחד.

## נושאים

- [[project-scaffold]] — סקירה כללית של מבנה הפרויקט הנוכחי
- [[claude-md]] — `CLAUDE.md` בשורש, התיאור הראשי של הפרויקט ל-Claude Code
- [[env-files]] — `.env` ו-`.env.example` (משתני סביבה)
- [[gitignore]] — `.gitignore` (חוסם קבצים מ-git)
- [[claude-code-config]] — `.claude/settings.json` (קונפיג Claude Code project-scoped)
- [[claude-subfolders]] — `.claude/{agents,skills,commands}/` (מבנה התת-תיקיות)
- [[obsidian-skills]] — שלוש ה-skills שמותקנות מקומית (`obsidian-vault-workflow`, `obsidian-markdown`, `obsidian-bases`)
- [[superpowers-plugin]] — plugin של `obra/superpowers` בהתקנת project scope
- [[skill-creator-plugin]] — plugin `skill-creator` (מ-`claude-plugins-official`) ליצירת skills חדשים
- [[git-and-github]] — git init, remote `origin`, היסטוריית קומיטים, GitHub

## איך להוסיף נושא חדש

1. לבחור שם קובץ kebab-case בלי תאריך, בלי `.md` בקישור
2. ליצור את הקובץ עם הפורמט בפרק "Topic file format" של `obsidian-vault-workflow/SKILL.md`
3. **להוסיף שורה כאן** עם `[[wikilink]]` ותיאור בשורה אחת
