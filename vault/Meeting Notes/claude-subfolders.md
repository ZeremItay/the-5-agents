---
tags:
  - config
  - claude-code
  - structure
status: wip
---

# .claude/ Subfolder Structure (`agents/`, `skills/`, `commands/`)

## Overview

תחת `.claude/` קיימות שלוש תת-תיקיות סטנדרטיות של Claude Code, כל אחת מיועדת לסוג שונה של הרחבה לפרויקט:

- **`.claude/agents/`** — הגדרות subagents (כל אחד קובץ `.md` עם YAML frontmatter שמגדיר מה הסוכן עושה ובאילו כלים)
- **`.claude/skills/`** — skills מותאמים לפרויקט (כל skill בתיקייה משלו עם `SKILL.md`)
- **`.claude/commands/`** — slash commands מותאמים (כל פקודה קובץ `.md`, נטענת כ-`/<filename>`)

כיום `agents/` ו-`commands/` ריקות (רק `.gitkeep`). `skills/` מכילה שלוש skills של Obsidian — ראה [[obsidian-skills]].

**משויך לכל מי שיגדיר subagents / skills / commands ייחודיים לפרויקט.**

## Open Questions

- מה תהיה הסט של agents ב-`agents/`? — מנכ"ל + אילו עוד?
- אילו slash commands שווה ליצור (`/brief`, `/publish`, `/review`...)? יוחלט כשיהיה תהליך עבודה ברור
- האם לכתוב skills חדשים ב-`skills/` (מעבר ל-Obsidian) או להסתפק ב-skills שמגיעים מ-superpowers?

## קבצים קשורים בפרויקט

- `.claude/agents/.gitkeep`
- `.claude/skills/.gitkeep`
- `.claude/commands/.gitkeep`
- `.claude/skills/obsidian-vault-workflow/SKILL.md`
- `.claude/skills/obsidian-markdown/SKILL.md`
- `.claude/skills/obsidian-bases/SKILL.md`

## Session Log

### 2026-05-05 — Subfolders created and skills added [wip]
- **What was done:** נוצרו שלוש התיקיות עם `.gitkeep` placeholders. המשתמש הוסיף ידנית שלוש skills של Obsidian ל-`skills/`.
- **Decisions:** placeholders של `.gitkeep` נחוצים כדי ש-git יעקוב אחרי תיקיות ריקות. ה-skills של Obsidian הוכנסו ידנית (לא דרך plugin) כי הם ספציפיים לפרויקט הזה.
- **Notes / Caveats:** סטטוס `wip` כי `agents/` ו-`commands/` עדיין ריקות — המבנה רק חצי-מאוכלס.
- **Related:** [[claude-code-config]], [[obsidian-skills]], [[superpowers-plugin]], [[project-scaffold]]
