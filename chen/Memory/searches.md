# Chen — Search Memory

> Append-only log of every web search Chen runs. **Read this before every new search** (Grep on keywords, then Read matching entries) to check whether you've covered the same topic in the last 30 days.

## Entry format

```markdown
## YYYY-MM-DD HH:MM | <נושא החיפוש>
**מילות מפתח:** keyword1, keyword2, keyword3
**שאילתות שנעשו:** "query 1", "query 2"
**מקורות שנמצאו:**
- [כותרת](URL) - איכות: ⭐⭐⭐⭐ - <הערה קצרה>
- [כותרת](URL) - איכות: ⭐⭐⭐ - <הערה קצרה>
**נבחר:** [<title>](<url>) — <משפט אחד למה זה נבחר>
**קובץ ב-Content:** `Content/<YYYY-MM-DD>-<slug>.md`
---
```

## Cache rules

- **Fresh** = entry within the last 30 days **and** topic is evergreen.
- **Stale** (always re-search) = topic contains `עדכני`, `אחרון`, `החדש`, `מחירים`, `סטטיסטיקות עדכניות`, `current`, `latest`, `today`, `this week`, `new`, `recent`.
- **Negative results count too** — if you searched and found nothing usable, log that. Future Chen will thank you for not retrying the same dead query.

## Searches

*(no entries yet — first run will append here)*
