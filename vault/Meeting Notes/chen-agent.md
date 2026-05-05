---
tags:
  - agent
  - research
  - web
status: shipped
---

# Chen — Web Research Agent

## Overview

Chen (חן) הוא הסוכן השלישי שמוגדר תחת [[reuven-orchestrator|Reuven]] — סוכנת מחקר ברשת שמטפלת בחיפוש, איתור מקורות אמינים, ושמירת מאמרים מהאינטרנט כקלט ל-pipeline של [[yael-agent|יעל]]. הכלים שלה: `WebSearch, WebFetch, Read, Write, Edit, Glob, Grep` (בלי Bash, בלי Task, בלי API חיצוני). הערך שלה מעל LLM רגיל: **מידע עכשווי, מקורות אמיתיים עם לינקים, אפס הזיות** — כל פיסת מידע שהיא שומרת נשענת על URL בפרונטמטר ובסוף הקובץ.

**Memory protocol:** Chen בודקת `chen/Memory/searches.md` לפני כל חיפוש (Grep על מילות מפתח → Read של ה-entries הרלוונטיים → שיפוט סמנטי). אם יש התאמה טרייה (≤30 ימים) ולא מדובר בנושא דינמי → מחזירה ל-Reuven שאלה אם להשתמש בקיים. אחרי כל חיפוש — מוסיפה entry בפורמט קבוע.

**Output:** Chen כותבת ל-`Content/<YYYY-MM-DD>-<slug>.md` (slug מכותרת המקור, kebab-case 3-6 מילים) עם YAML frontmatter (`source_url`, `source_title`, `source_publisher`, `source_published`, `fetched_at`, `language`, `quality_rating`) + body מלא של המאמר + footer עם קישור מסכם. הפורמט מאפשר ל-Yael לייחס את המקור בשכתוב.

**אילוץ ארכיטקטוני:** Chen לא קוראת ל-Yael ישירות (סאב-אייג'נטים ב-Claude Code לא מפעילים אחד את השני). היא רק מכינה את הקרקע ב-`Content/` ומדווחת ל-Reuven. **Reuven** הוא זה שמחליט: אם הבקשה המקורית של המשתמש כללה פועל מפורש לשכתוב (`שכתב|תכתוב|תשכתב|רואיין|rewrite|write\s+about|prepare\s+article|תכין מאמר`) — Reuven ממשיך אוטומטית להפעיל את Yael על הקובץ; אחרת — Reuven עוצר ומחזיר את דיווח Chen למשתמש.

**משויך:** סאב-אייג'נט של [[reuven-orchestrator|Reuven]]. נטען על ידי Claude Code מ-`.claude/agents/chen.md`.

## Open Questions

- **Memory growth strategy** — `chen/Memory/searches.md` הוא append-only וללא archival. מתי לעבור ל-rotation לפי תאריך (למשל `searches-2026-Q2.md`)? כרגע: כשהקובץ יעבור 5000 שורות.
- **Trigger keyword conflicts** — "מקור על", "article about", "find" — חופפים חלקית עם yael ("מאמר", "article"). צפוי שניתוב לא חד־משמעי בבקשות מעורבות (למשל "מצא ותכתוב מאמר על X"). הפתרון הנוכחי: Reuven מנתב ל-Chen קודם, ואז ה-Post-Chen Auto-Chain מזהה את הפועל "תכתוב" ומשרשר ליעל. נטר ב-CEO-Logs.
- **Multi-source mode** — כברירת מחדל Chen בוחרת מקור אחד. כשהמשתמש כותב "מקיף"/"comprehensive"/"כמה מקורות" — היא יוצרת מספר קבצים `<slug>-1`, `<slug>-2`. האם Yael יכולה למזג מספר קבצים לשכתוב אחד? כרגע לא — היא מטפלת בקובץ אחד בכל ריצה. ייתכן שנצטרך mode חדש.
- **Paywall handling** — אם המקור הכי איכותי מאחורי paywall, Chen מנסה את הבא בתור או חוזרת עם caveat. האם להוסיף flag `paywalled: true` ב-frontmatter כדי שיעל תדע שהתוכן חלקי? כרגע Chen פשוט מציינת ב-quality_rating ובדיווח.
- **Auto-chain fallthrough** — אם Reuven מזהה פועל שכתוב והפעיל את Yael, אבל Yael נכשלת (קובץ ריק, fetch חלקי) — מה הזרם? כרגע Reuven מחזיר את שתי הדיווחים למשתמש; אין retry אוטומטי.
- **Hebrew vs English preference logic** — כרגע: עברית כשהנושא ישראלי או הבקשה בעברית; אנגלית default. עדיין לא מוגדר מה קורה לנושאים גלובליים שיש להם כיסוי טוב בעברית (למשל סקירת AI ב-Calcalist).

## קבצים קשורים בפרויקט

- `.claude/agents/chen.md` — הגדרת הסוכנת הקנונית (frontmatter, system prompt, workflow, hard rules)
- `chen/` (שורש) — תיקיית עבודה (`agent.md` pointer + `Memory/searches.md` log)
- `Content/` — תיקיית ה-pipeline שגם [[yael-agent|Yael]] קוראת ממנה
- `.claude/agents/reuven.md` — Reuven שמנתב משימות מחקר ל-Chen ומפעיל Auto-Chain ל-Yael כשרלוונטי
- `chen/Memory/searches.md` — לוג חיפושים, נכתב ב-runtime

## Session Log

### 2026-05-05 — Chen scaffolded as agent-3 [shipped]
- **What was done:** הוטמעו (1) `.claude/agents/chen.md` עם frontmatter (`tools: WebSearch, WebFetch, Read, Write, Edit, Glob, Grep` — בלי Bash, בלי Task, בלי API חיצוני; `model: sonnet`) ו-body מפורט: Role, Working Directories, Workflow (8 שלבים: memory-check → WebSearch → quality-filter → WebFetch → choose-source → save-to-Content → log-to-Memory → report), Hard Rules, Reference. (2) תיקייה `chen/` בשורש עם `agent.md` (pointer) ו-`Memory/searches.md` (template עם entry format + cache rules). (3) עדכון [[reuven-orchestrator|Reuven]] להוסיף את Chen כ-agent-3 בטבלת ה-routing, להוסיף סעיף "Post-Chen Auto-Chain" ב-Decision Protocol, ולעדכן Status ל-"3 of 4 — Partially operational".
- **Decisions:** **Memory protocol היברידי** (Grep + Read + שיפוט סמנטי) ולא literal-match בלבד — קל ל-implementation והנושא קטן מספיק שזה לא יקר. **30-day cache window** עם override אוטומטי ל-dynamic topics ("עדכני", "latest" וכו'). **YAML frontmatter בקובץ ה-Content/`** עם 7 שדות (source_url, source_title, source_publisher, source_published, fetched_at, language, quality_rating) — מאפשר ל-Yael לייחס את המקור בשכתוב, ול-Obsidian להציג properties. **Auto-chain רק כשיש פועל מפורש לשכתוב** ("תכתוב", "תשכתב", "rewrite", "write about", "prepare article", "תכין מאמר"); "חפש"/"מצא"/"find"/"search"/"research" לבד → עוצר אחרי Chen. **Slug מכותרת המקור** (3-6 kebab-case words) ולא מהבקשה — מידעי ומזהה את המקור. **Single-source default** עם multi-source mode כשהמשתמש מבקש מפורשות. **Chen לא מוחקת קבצים** (אין לה Bash) — אבל היא רק מוסיפה ל-`Content/`, אז אין מה למחוק; ההפרש מ-Yael שיוצרת files ב-Content/Ready/ בכל מקרה.
- **Notes / Caveats:** ה-trigger keywords של Chen ("מקור על", "article about") חופפים עם Yael; ייתכן ש-Reuven ינתב לפעמים ל-Yael כשהיה צריך ל-Chen, או להפך. ה-Post-Chen Auto-Chain בנוי כשכבת תיקון: גם אם הניתוב הראשוני שגוי, ה-pipeline המלא (Chen → Yael) יקרה אם הבקשה כוללת שני המרכיבים. ה-Memory file מתחיל append-only — נשקול archival כשיגיע ל-5000+ שורות. שירות ה-WebSearch של Claude Code לא תמיד נותן תוצאות עדכניות; Chen מומלצת להשתמש ב-`site:` qualifiers ובמילות מפתח ספציפיות.
- **Related:** [[reuven-orchestrator]], [[yael-agent]], [[yuval-agent]], [[claude-subfolders]], [[claude-md]]
