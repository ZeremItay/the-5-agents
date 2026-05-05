---
tags:
  - plugin
  - claude-code
  - skills
  - tooling
status: shipped
---

# Skill Creator Plugin (`skill-creator@claude-plugins-official`)

## Overview

Plugin של Claude Code שמספק skill בשם `skill-creator` — מדריך מקצה-לקצה ליצירה, עריכה, אופטימיזציה ובדיקת skills חדשים. מקור הקוד הקנוני הוא [anthropics/skills/skills/skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator), אבל ההתקנה כאן מגיעה מ-marketplace `claude-plugins-official` שמשקף את אותו תוכן ומאפשר להתקין אותו כ-plugin עצמאי (ה-marketplace `anthropic-agent-skills` חושף רק חבילות מקובצות כמו `example-skills` ו-`document-skills`). הותקן ב-**project scope** במקביל ל-user scope קיים.

**משויך לכל סוכן/מפתח שמתחזק ויוצר skills חדשים בפרויקט הזה — בעיקר רלוונטי כשנרצה להוסיף skills ל-`.claude/skills/` (למשל skill ייחודי לסוכן מנכ"ל, skill לזרימת publish, skill לבריאוּת תוכן).**

## Open Questions

- האם `claude-plugins-official` באמת מסונכרן עם `anthropics/skills` upstream, או שיש drift (גרסה שונה / תכונות חסרות)? לבדוק אם ייווצר חוסר התאמה
- האם להעדיף בעתיד להעתיק את התוכן ידנית ל-`.claude/skills/skill-creator/` (כמו ה-obsidian skills) במקום plugin install? יתרון: גרסה stable; חיסרון: פספוס updates
- אילו skills נכתוב לפרויקט הזה בפועל? עד שאין רשימה — ה-plugin הזה לא משמש בייצור

## קבצים קשורים בפרויקט

- `.claude/settings.json` — רישום ה-plugin תחת `enabledPlugins`
- (התוכן עצמו של ה-skill לא יושב בריפו — הוא נטען מה-cache של Claude Code לכל user שמפעיל את ה-plugin)

## Session Log

### 2026-05-05 — Installed skill-creator at project scope [shipped]
- **What was done:** הריצנו `claude plugin install skill-creator@claude-plugins-official --scope project`. תחילה ניסינו `skill-creator@anthropic-agent-skills` (ה-marketplace שמיוצג ע"י ה-URL שהמשתמש שיתף), אבל קיבלנו `Plugin "skill-creator" not found in marketplace "anthropic-agent-skills"` כי שם זה מסונן רק ל-`document-skills`/`example-skills`. עברנו ל-`claude-plugins-official` שכן חושף אותו ככזה.
- **Decisions:** project scope (לא user) כדי לשמור על אחידות עם שאר ה-plugins של הפרויקט (ראה [[superpowers-plugin]]). השארנו את ה-user-scope המקביל פעיל — אין סיבה לכבות אותו.
- **Notes / Caveats:** דורש restart של Claude Code (או `/hooks`-style רענון של config) כדי שה-skill ייטען. ה-version מסומן `unknown` ב-`claude plugin list` — לא נראה שיש פיצ'רינג מסודר ב-marketplace הזה.
- **Related:** [[claude-code-config]], [[superpowers-plugin]], [[claude-subfolders]], [[obsidian-skills]], [[project-scaffold]]
