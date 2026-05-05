---
tags:
  - agent
  - orchestrator
  - architecture
status: shipped
---

# Reuven — CEO Orchestrator

## Overview

Reuven הוא סוכן המנכ"ל (CEO orchestrator) של מערכת `TESTTOMORROW`. הוא נקודת הכניסה היחידה לכל משימה: מקבל את הבקשה, בוחר סאב-אייג'נט אחד מתוך 4 על בסיס domain + trigger keywords, מפעיל אותו דרך Task tool של Claude Code, מתעד את ההחלטה והתוצאה ב-[[CEO-Logs/_index|vault/CEO-Logs/]], ומחזיר למשתמש סיכום מסונתז. נכון ל-2026-05-05 הוא **3/4 מבצעי** — שלושה סאב-אייג'נטים מוגדרים: [[yuval-agent|יובל]] (קריאייטיב/תמונות), [[yael-agent|יעל]] (כותבת תוכן, LLM-only), ו-[[chen-agent|חן]] (חוקרת רשת, WebSearch+WebFetch) — אחד נוסף עדיין TBD. בקשות ויזואליות → Yuval; בקשות שכתוב/עריכה/תרגום/סיכום → Yael (עם post-pipeline אוטומטי שמחליף `{{IMAGE_NEEDED}}` placeholders ב-image references אמיתיים ע"י קריאה ל-Yuval); בקשות חיפוש/מחקר ברשת → Chen (עם Post-Chen Auto-Chain ל-Yael אם הבקשה המקורית של המשתמש כללה פועל מפורש לשכתוב); היתר → Out-of-Scope ומתועד.

**משויך:** Claude Code session-level — כל סשן בפרויקט עוקב אחר הוראות [[claude-md|CLAUDE.md]] שמטעינות את Reuven כ-default entry point.

## Open Questions

- **הסאב-אייג'נט הנותר** (agent-4) — שם, תחום אחריות, trigger keywords, יכולות. Open until next planning session
- **טבלת ה-Routing הסופית** — תושלם כשיוגדר agent-4. כרגע yuval=agent-1, yael=agent-2, chen=agent-3
- **התנגשויות trigger keywords** — בעיקר בין yael ל-chen ("מאמר"/"article" משותף). הפתרון הנוכחי: Reuven מנתב לפי הפועל (חפש→Chen, שכתב→Yael), והמנגנון Post-Chen Auto-Chain מתקן בקשות מעורבות. נטר ב-[[CEO-Logs/_index|CEO-Logs]]
- **רובוסטיות ה-`rm` ב-Post-Yael Cleanup** — Reuven מבצע `rm Content/<name>.md` אחרי שיעל מסיימת; אם Reuven נופל באמצע, נשאר כפיל. כרגע Read-check על `Content/Ready/` מקטין סיכון אבל לא אטומי
- **Auto-chain failure mode** — אם Chen מצליחה ויעל נכשלת, Reuven מחזיר את שתי הדיווחים למשתמש. אין retry אוטומטי. תלוי בנתוני שימוש
- **Multi-source mode** של Chen → Yael — Chen יכולה ליצור כמה קבצים `<slug>-1`, `<slug>-2`. Yael כרגע מטפלת בקובץ אחד בכל ריצה. ייתכן שנצטרך merge mode
- האם יש צורך ב-CLAUDE.md ספציפי לכל סאב-אייג'נט בעתיד?
- **Retry logic** מתקדם יותר (כרגע: max 1 retry per agent) — תלוי בנתוני שימוש בפועל
- **Parallel execution** לסוכנים שלא חופפים — מסומן עתידי ב-PRD סעיף 8
- **Task decomposition** (Reuven שובר משימה מורכבת לתת-משימות) — מסומן עתידי ב-PRD סעיף 8

## קבצים קשורים בפרויקט

- `CLAUDE.md` (שורש) — מטעין את Reuven כ-CEO ב-session start (ראה [[claude-md]])
- `.claude/agents/reuven.md` — הגדרת הסוכן המלאה (frontmatter, system prompt, hard rules, logging format)
- `vault/CEO-Logs/` — תיקיית הלוגים שבה Reuven כותב entries ב-runtime (ראה [[CEO-Logs/_index|CEO Logs]])

## Session Log

### 2026-05-05 — Reuven scaffolded per PRD v1.0 [shipped]
- **What was done:** הוטמעו 6 קבצים לפי PRD Reuven v1.0: (1) `merge` של [[claude-md|CLAUDE.md]] עם בלוק "CEO Orchestrator — Activation Protocol" באנגלית בראש (התיאור הקיים בעברית נשמר). (2) קובץ סוכן חדש `.claude/agents/reuven.md` עם YAML frontmatter (`name: reuven`, `tools: Task,Read,Write,Edit,Glob,Grep`, `model: sonnet`) + body מלא לפי PRD סעיף 6 (Role, Decision Protocol, Hard Rules, Logging Format, Out-of-Scope, Failure Handling, Status). (3) `vault/CEO-Logs/_index.md` חדש — folder index שמסביר את הפורמט. (4) עדכון `vault/_index.md` להוסיף את התיקייה החדשה. (5) topic file הזה. (6) עדכון `vault/Meeting Notes/_index.md` להוסיף את הנושא הזה.
- **Decisions:** **CEO-Logs ב-vault/ בריפו** (לא חיצוני) — נשמר עם הפרויקט, עקבי עם שאר ה-vault, ועובר עם clone. **CLAUDE.md merge ולא replace** — שומר על התיאור העברי של הפרויקט שהיה שם, מוסיף בלוק activation בראש שדומיננטי ל-Claude. **Placeholders + TODO ברורים בקובץ הסוכן** (לא defer) — הסקאפולד מוכן להתחבר לסוכנים ברגע שיוגדרו, בלי drift עם ה-PRD; ה-`> ⚠️ Status: TBD` callout בולט מספיק כדי שלא יישכח.
- **Notes / Caveats:** כל המשימות יחזרו כ-`Out-of-Scope` עד שיוגדרו 4 הסאב-אייג'נטים — זה לא bug, זה ה-state המתוכנן של PRD v1.0. ה-`description` ב-frontmatter תורגם ל"single entry point for the multi-agent system" כדי לעודד את Claude Code לבחור ב-Reuven default על משימות שמגיעות. בחרנו `model: sonnet` (לא opus) כי PRD מציין שזה מספיק לניתוב — sonnet 4.6 הוא הברירת מחדל היעילה כאן.
- **Related:** [[claude-md]], [[claude-subfolders]], [[claude-code-config]], [[project-scaffold]], [[CEO-Logs/_index|CEO Logs]]

### 2026-05-05 — Roster updated: yuval added as agent-1 [shipped]
- **What was done:** סאב-אייג'נט ראשון התווסף — [[yuval-agent|יובל]], סוכן קריאייטיב ליצירת תמונות. עודכן `.claude/agents/reuven.md`: (1) ב-"Sub-Agents Under Your Command" שונה `<agent-1>` ל-yuval מלא עם domain + trigger keywords (תמונה של, ציור של, generate image וכו'); (2) ה-callout שונה מ-"Status: TBD" ל-"Partial roster — 1 of 4"; (3) סעיף "Status" בסוף עודכן ל-"🟡 Partially operational" עם הערה על תאריך ה-roster update.
- **Decisions:** **Yuval = agent-1** (לא agent-2/3/4) כי הוא הסוכן הראשון שמוגדר וזה המיקום הטבעי. שמרנו את 3 הסלוטים הנותרים TBD ולא מחקנו את ה-Out-of-Scope flow כי הוא עדיין רלוונטי לרוב המשימות. ה-trigger keywords בעברית ובאנגלית כדי שReuven ינתב גם מבקשות עבריות ('תמונה של...') וגם אנגליות ('generate an image').
- **Notes / Caveats:** Yuval הוגדר במקביל לסקיל [[gpt-image-gen-skill]] שהוא משתמש בו. ה-`OPENAI_API_KEY` נוסף ל-`.env.example` ול-`.env` — חייב להיות מוגדר ידנית לפני שYuval יתחיל לפעול. ה-trigger keywords עשויים להזדקק לליטוש אחרי כמה ניתובים בפועל — נראה דפוסים ב-CEO-Logs.
- **Related:** [[yuval-agent]], [[gpt-image-gen-skill]], [[claude-subfolders]], [[env-files]]

### 2026-05-05 — Roster updated: yael added as agent-2 + Post-Yael pipeline protocol [shipped]
- **What was done:** סאב-אייג'נט שני התווסף — [[yael-agent|יעל]], סוכנת תוכן (LLM-only). עודכן `.claude/agents/reuven.md` ב-3 מקומות: (1) ב-"Sub-Agents Under Your Command" שונה `<agent-2>` ל-yael מלא עם domain + trigger keywords (שכתב, ערוך, נסח מחדש, תרגם, סכם, מאמר, תוכן, פוסט / rewrite, edit, rephrase, translate, summarize, article, content, post); ה-callout שונה מ-"1 of 4" ל-"2 of 4". (2) נוסף סעיף חדש "Post-Yael Cleanup & Image Pipeline" בין Hard Rules ל-Logging Format — זה מגדיר את שני הצעדים שReuven חייב לבצע אחרי שיעל מחזירה: (a) **מחיקת המקור מ-`Content/`** (יעל לא יכולה כי אין לה Bash) — Read-check על `Content/Ready/<name>.md` ואז `rm`; (b) **Image pipeline** — parse של `{{IMAGE_NEEDED: "..."}}` placeholders, קריאה ליובל לכל אחד דרך Task tool, החלפת ה-placeholder ב-`![alt](yuval/outputs/path.png)`, ושמירה חזרה ל-`Output/<name>.md`. (3) Hard Rules עודכנו עם שורה חדשה: "When the routed agent is yael, always run Post-Yael Cleanup & Image Pipeline". (4) סעיף "Status" בסוף עודכן ל-"2 of 4" עם הערה על pipeline ה-Post-Yael האוטומטי.
- **Decisions:** **Yael = agent-2** כי היא הסוכנת השנייה שמוגדרת. שמרנו את 2 הסלוטים הנותרים TBD. **ה-Post-Yael pipeline ב-Reuven (לא ביעל)** כי סאב-אייג'נטים ב-Claude Code לא יכולים להפעיל אחד את השני — רק Reuven (שיש לו Task) מתאים. **`{{IMAGE_NEEDED: "..."}}` syntax** עם regex מדויק (`\{\{IMAGE_NEEDED:\s*"[^"]+"\}\}`) — קל ל-grep ולהחלפה, מובחן מ-Markdown סטנדרטי. **המחיקה מ-`Content/` באחריות Reuven** ולא יעל — יעל אין לה Bash, ובנוסף זה תפקיד orchestration טבעי. **כל קריאה לסוכן (yael / yuval) → entry נפרד ב-CEO-Log** כדי שניתן יהיה לעקוב אחרי pipeline-ים מורכבים. **Image path: root-relative** (`yuval/outputs/...`) במקום relative-to-Output כי זה יציב כשהקובץ נצרך מ-IDE/Obsidian/git.
- **Notes / Caveats:** ה-trigger keywords של yael גנריים יותר מאלה של yuval ("תוכן"/"content"/"edit") — אם יתווסף בעתיד agent של "code editor" או "publishing" ייתכן ניגוד; נטר את [[CEO-Logs/_index|CEO-Logs]]. ה-rm ב-step (1) של ה-Post-Yael flow לא אטומי — אם Reuven נופל באמצע (אחרי שיעל סיימה אבל לפני ה-rm) נשאר כפיל ב-`Content/` ו-`Content/Ready/`; ה-Read-check על `Content/Ready/<name>.md` רק מקטין סיכון. ה-image pipeline סדרתי — אם יש 5 placeholders, זה 5 קריאות ל-OpenAI דרך Yuval. ה-`description` של yael ב-frontmatter ארוך יחסית — מכוון, כדי לעודד את Reuven לבחור בה לבקשות תוכן מגוונות.
- **Related:** [[yael-agent]], [[yuval-agent]], [[gpt-image-gen-skill]], [[claude-subfolders]], [[claude-md]]

### 2026-05-05 — Roster updated: chen added as agent-3 + Post-Chen Auto-Chain protocol [shipped]
- **What was done:** סאב-אייג'נט שלישי התווסף — [[chen-agent|חן]], חוקרת רשת (WebSearch + WebFetch). עודכן `.claude/agents/reuven.md` ב-3 מקומות: (1) ב-"Sub-Agents Under Your Command" שונה `<agent-3>` ל-chen מלא עם domain + trigger keywords (חפש, מצא, מחקר, מאמר על, חדש על, מה קורה עם, מקור על / search, find, research, article about, latest on, news on); ה-callout שונה מ-"2 of 4" ל-"3 of 4". (2) נוסף סעיף חדש "Post-Chen Auto-Chain" אחרי Post-Yael. שלושה צעדים: log תמיד; בדיקה רגקסית של הבקשה המקורית לפועל מפורש לשכתוב (`שכתב|תשכתב|תכתוב\s+מאמר|תכין\s+מאמר|רואיין|נסח|rewrite|write\s+about|prepare\s+article|draft\s+(an\s+)?article|turn\s+.*\s+into`); אם נמצא → השרשור אוטומטית ל-Yael (שמפעיל בתורו את ה-Post-Yael flow כולל Yuval); אם לא → עוצר אחרי Chen ומחזיר את הדיווח שלה. case מיוחד: אם Chen מחזירה "כבר חיפשתי, רוצה את הקיים?" — Reuven מעביר את השאלה למשתמש בלי לשרשר. (3) Hard Rules עודכנו עם שורה חדשה לטיפול ב-Post-Chen. (4) סעיף "Status" בסוף עודכן ל-"3 of 4".
- **Decisions:** **Chen = agent-3** עם trigger keywords חופפים חלקית עם yael (משותף: "מאמר", "article"). הבחירה: לא להגדיר keywords לעומק כדי למנוע over-fitting; במקום זה, ה-Post-Chen Auto-Chain מתקן ניתוב שגוי בבקשות מעורבות. **Auto-chain trigger רק על פעלים מפורשים לשכתוב** ("תכתוב", "תשכתב", "rewrite", "write about", "prepare article", וגם "draft an article", "turn ... into"). פעלי חיפוש לבד ("חפש", "מצא", "find", "research") עוצרים אחרי Chen. **שרשור מלא Chen → Yael → Yuval** אפשרי בריצה אחת, כי ה-Post-Yael הוא חלק מהפרוטוקול הסטנדרטי שמופעל אוטומטית כש-Reuven מפעיל את Yael. **CEO-Log entry נפרד לכל שלב בשרשור** — מאפשר ניטור של pipeline-ים מורכבים. **Special case לחיפוש כפול:** אם Chen מזהה התאמת cache, היא מחזירה שאלה במקום לחפש מחדש; Reuven מעביר את השאלה למשתמש בלי לשרשר — שומר על השליטה של המשתמש בקאש.
- **Notes / Caveats:** ה-WebSearch של Claude Code לא תמיד עדכני; Chen צריכה להשתמש ב-`site:` qualifiers ובמילות מפתח ספציפיות כדי לקבל תוצאות איכותיות. ה-Memory file מתחיל append-only — צפי לחפיף ל-archival כשיגיע ל-5000+ שורות. בקשות מעורבות ("מצא ותכתוב מאמר על X") עכשיו זורמות נכון: Chen מנותבת ראשונה, ה-Auto-Chain מזהה "תכתוב" ומשרשר ליעל, Post-Yael מטפל בתמונות. אם Chen מחזירה multi-source (`<slug>-1`, `<slug>-2`), כרגע Reuven משרשר את הראשון ל-Yael; merge mode הוא Open Question.
- **Related:** [[chen-agent]], [[yael-agent]], [[yuval-agent]], [[gpt-image-gen-skill]], [[claude-md]]
