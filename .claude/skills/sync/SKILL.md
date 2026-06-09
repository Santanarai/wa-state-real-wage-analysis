---
name: sync
description: Mid-session Notion refresh — pulls new strategic updates without re-reading project files
disable-model-invocation: true
---
Read the Notion project hub (read URL from `.claude/config.local` as `NOTION_HUB_URL`) for any new methodology decisions or strategic updates. This is a mid-session context update, not a new session — do not re-read CLAUDE.md or SESSION_STATE.md, and do not reset your understanding of what has been accomplished in the current session. Report any new decisions that affect the current work, then continue.
