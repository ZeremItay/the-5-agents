---
tags:
  - plugin
  - claude-code
  - skills
status: shipped
---

# Superpowers Plugin (`obra/superpowers`)

## Overview

Plugin של Claude Code מ-[obra/superpowers](https://github.com/obra/superpowers), הותקן ב-**project scope** (לא user) דרך `claude plugin install superpowers@superpowers-dev`. הוא מספק חבילה של skills מובנים לזרימות עבודה: `brainstorming`, `writing-plans`, `executing-plans`, `systematic-debugging`, `test-driven-development`, `verification-before-completion`, `using-git-worktrees`, ועוד. בנוסף הוא טוען את `using-superpowers` כ"דלת כניסה" — skill שמסביר איך למצוא ולהשתמש ב-skills.

**משויך לכל סוכן Claude Code שעובד בפרויקט הזה — כל ה-skills זמינים אוטומטית.**

## Open Questions

- האם להגדיר default workflow מועדף לסוכנים (TDD? brainstorming-first?) דרך `CLAUDE.md` או hook?
- האם להחזיק גם את `superpowers@claude-plugins-official` ברמת user, או לכבות אותו כדי שלא יתנגש עם ה-project version?
- האם נוסיף עוד plugins (למשל `claude-md-management`, `frontend-design`) ב-project scope?

## קבצים קשורים בפרויקט

- `.claude/settings.json` — ה-marketplace וה-plugin מוצהרים שם

## Session Log

### 2026-05-05 — Installed at project scope [shipped]
- **What was done:** `claude plugin marketplace add obra/superpowers --scope project` הוסיף את ה-marketplace בשם `superpowers-dev`. אחר כך `claude plugin install superpowers@superpowers-dev --scope project` התקין את ה-plugin (גרסה 5.1.0).
- **Decisions:** עדיף ה-upstream (`obra/superpowers`) על המראה הרשמית (`anthropics/claude-plugins-official`) כדי לקבל את הגרסה החדשה ביותר. project scope ולא user scope כדי שההתקנה תיסע יחד עם הריפו.
- **Notes / Caveats:** דורש restart של Claude Code כדי שה-skills, agents, ו-slash commands של ה-plugin ייטענו במלואם. גם הגרסה הישנה יותר (`@claude-plugins-official` v5.0.7) עדיין מותקנת ב-user scope אצל המשתמש.
- **Related:** [[claude-code-config]], [[claude-subfolders]], [[project-scaffold]]
