---
tags:
  - skills
  - obsidian
  - documentation
status: shipped
---

# Obsidian Skills (`obsidian-vault-workflow`, `obsidian-markdown`, `obsidian-bases`)

## Overview

שלוש skills שהמשתמש הוסיף ידנית תחת `.claude/skills/`, כל אחת בתיקייה משלה עם `SKILL.md`:

- **`obsidian-vault-workflow`** — *הפרוטוקול המחייב* של קריאה/כתיבה ל-vault של הפרויקט. מגדיר את מבנה התיקיות (Meeting Notes / Content Briefs / Publishing Log / Brand Guidelines), את פורמט קובץ-נושא (Overview / Open Questions / Session Log), ואת ה-anti-patterns.
- **`obsidian-markdown`** — תחביר Obsidian Flavored Markdown: wikilinks (`[[...]]`), embeds (`![[...]]`), callouts (`> [!type]`), frontmatter, tags, footnotes, math, mermaid.
- **`obsidian-bases`** — קבצי `.base` של Obsidian (תצוגות מסד נתונים: tables, cards, filters, formulas, summaries).

**משויך לכל סוכן שכותב או קורא תיעוד ב-`vault/`.** ה-skill הראשון הוא ה"חוק" — חייב לרוץ בתחילת ובסוף כל משימה.

## Open Questions

- האם נכתוב skill נוסף שמייצר automatic templates של topic files חדשים?
- האם להעביר את ה-skills האלה ל-plugin עצמאי כדי שיהיה קל לשתף בין פרויקטים? (כרגע הם מקומיים בלבד)

## קבצים קשורים בפרויקט

- `.claude/skills/obsidian-vault-workflow/SKILL.md`
- `.claude/skills/obsidian-markdown/SKILL.md`
- `.claude/skills/obsidian-bases/SKILL.md`

## Session Log

### 2026-05-05 — Skills loaded and protocol activated [shipped]
- **What was done:** המשתמש הוסיף את שלוש ה-skills ידנית. הסשן הראשון שאחרי-ההוספה (זה) קרא את `obsidian-vault-workflow/SKILL.md` באופן מלא ויישם אותו כדי לאתחל את ה-vault.
- **Decisions:** הוחלט להתחיל רק עם תיקיית `Meeting Notes/` (כל הנושאים הראשוניים נופלים תחת ארכיטקטורה/תשתית); שאר התיקיות יוקמו on-demand. נכתב כל ה-vault בעברית כדי להתאים לסגנון של [[claude-md]].
- **Notes / Caveats:** ה-skills נוספו אחרי תחילת הסשן ולכן לא הופיעו ברשימת ה-skills האוטומטית של ה-system reminder — ראינו אותן רק אחרי ש-listed את התיקייה ידנית.
- **Related:** [[claude-subfolders]], [[claude-code-config]], [[project-scaffold]]
