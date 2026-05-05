---
tags:
  - config
  - claude-code
  - plugins
status: shipped
---

# .claude/settings.json (Project-Scoped Claude Code Config)

## Overview

`.claude/settings.json` הוא קובץ ההגדרות של Claude Code ברמת **הפרויקט** — נטען אוטומטית כשפותחים את התיקייה ב-Claude Code. כיום הוא מכיל שלושה מפתחות: `extraKnownMarketplaces` (רושם את ה-marketplace `superpowers-dev` שמצביע ל-`obra/superpowers` ב-GitHub), `enabledPlugins` (מפעיל את `superpowers@superpowers-dev`), ו-`hooks` (שני hooks — `SessionStart` ו-`UserPromptSubmit` — שמזריקים תזכורת על פרוטוקול [[obsidian-skills|obsidian-vault-workflow]]). בעתיד הוא עשוי לקבל `permissions`, `env`, ועוד.

**משויך לכל סוכן Claude Code שיעבוד בפרויקט הזה.**

## Open Questions

- האם להגביל permissions/allowlists ב-`.claude/settings.json` או להשאיר ל-user settings?
- האם נחזיק שני קבצים נפרדים (`settings.json` משותף + `settings.local.json` אישי לכל מפתח)?
- האם הטקסט של ה-additionalContext ב-hooks מספיק ברור, או שצריך להאריך/לקצר עם הניסיון?

## קבצים קשורים בפרויקט

- `.claude/settings.json`

## Session Log

### 2026-05-05 — Plugin marketplace + plugin enabled at project scope [shipped]
- **What was done:** הריצונו `claude plugin marketplace add obra/superpowers --scope project` ו-`claude plugin install superpowers@superpowers-dev --scope project`. הקובץ נוצר אוטומטית עם `extraKnownMarketplaces` ו-`enabledPlugins`.
- **Decisions:** scope `project` (לא `user`) כדי שכל מי שיעשה clone יקבל את אותה התקנה. בחרנו ב-`obra/superpowers` upstream במקום `claude-plugins-official` כדי לקבל את הגרסה החדשה ביותר (5.1.0 לעומת 5.0.7).
- **Notes / Caveats:** ה-skill files של ה-plugin נטענים רק אחרי restart של Claude Code. ה-user-scope של `superpowers@claude-plugins-official` עדיין מותקן במקביל — לא הוסר.
- **Related:** [[superpowers-plugin]], [[claude-subfolders]], [[project-scaffold]]

### 2026-05-05 — Added SessionStart + UserPromptSubmit hooks for vault protocol [shipped]
- **What was done:** הוספו שני hooks לקובץ: `SessionStart` (פעם בתחילת כל סשן) ו-`UserPromptSubmit` (לפני כל prompt של משתמש). שניהם מריצים `echo` עם JSON שמזריק `additionalContext` המסביר ל-Claude את הפרוטוקול של [[obsidian-skills|obsidian-vault-workflow]] — מה לקרוא לפני המשימה, מה לעדכן אחרי. ה-hook משתמש ב-`shell: bash` כדי לעבוד גם ב-Git Bash על Windows וגם ב-bash native על macOS/Linux.
- **Decisions:** שני hooks ולא רק SessionStart, כי SessionStart יכול "להתפספס" כשהקונטקסט מתמלא; UserPromptSubmit מבטיח שכל prompt מקבל תזכורת עדכנית. בחרנו תוכן shorter ל-UserPromptSubmit ו-longer ל-SessionStart כדי לא להטיל הרבה context overhead על כל prompt. בחרנו הזרקת JSON עם `hookSpecificOutput.additionalContext` (לא plain text) כדי שהתזכורת באמת תיכנס ל-system reminder של המודל.
- **Notes / Caveats:** ה-hooks ייכנסו לתוקף רק ב-(א) הסשן הבא של Claude Code, או (ב) אחרי שהמשתמש פותח את `/hooks` בסשן הנוכחי (הפעולה הזאת מרעננת את ה-watcher של ה-config). אומת ב-pipe test שהפקודות של שני ה-hooks מפיקות JSON תקין באורכים סבירים (SessionStart=678 chars, UserPromptSubmit=189 chars).
- **Related:** [[obsidian-skills]], [[claude-subfolders]], [[project-scaffold]], [[superpowers-plugin]]

### 2026-05-05 — Installed skill-creator plugin at project scope [shipped]
- **What was done:** הוסף `skill-creator@claude-plugins-official` ל-`enabledPlugins` תחת project scope. הקובץ עודכן ע"י ה-CLI עצמו (`claude plugin install --scope project`) — לא נכתב ידנית. סדר המפתחות ב-`settings.json` השתנה (כעת `hooks` ראשון, אחר כך `enabledPlugins`, אחר כך `extraKnownMarketplaces`) — זה side effect של הסריאליזציה של Claude Code, לא משפיע על תפקוד.
- **Decisions:** המשתמש ביקש להתקין מ-URL של `anthropics/skills`, אבל ה-marketplace `anthropic-agent-skills` (שזה שלו) חושף את `skill-creator` רק כתת-skill בתוך `example-skills`/`document-skills`, לא כ-plugin עצמאי. בחרנו להתקין מ-`claude-plugins-official` שכן חושף ככזה — שני המקורות מתוחזקים ע"י Anthropic.
- **Notes / Caveats:** אומת ב-`claude plugin list` שהפלאגין מופיע ב-project scope. ה-hooks וה-marketplace `superpowers-dev` נשמרו במלואם.
- **Related:** [[skill-creator-plugin]], [[superpowers-plugin]], [[claude-subfolders]], [[project-scaffold]]
