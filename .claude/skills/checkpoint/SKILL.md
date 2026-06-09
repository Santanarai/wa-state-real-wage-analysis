---
name: checkpoint
description: Mid-session progress sync to Notion — does not update SESSION_STATE.md or commit
disable-model-invocation: true
---
Update the Notion project hub session log (read URL from `.claude/config.local` as `NOTION_HUB_URL`) with a brief progress note. Add a row to the Session Log table with today's date, what has been accomplished so far this session, and any decisions or issues that have come up. This is a mid-session checkpoint, not a session close — do not update SESSION_STATE.md or commit code. Just sync progress to Notion and continue working.
