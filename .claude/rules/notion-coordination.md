# Notion Coordination Rule

Do NOT update the Notion project hub automatically or as part of task
execution. Notion is the canonical shared record and is updated by the
strategy thread (Claude.ai) after quality review.

Exceptions (user-initiated commands only):
- /checkpoint — mid-session sync to Notion. Does NOT update SESSION_STATE.md.
- /wrapup — session-end update to both SESSION_STATE.md and Notion.

Automatic Notion updates during task execution (e.g., after completing a
section) are not permitted without explicit user instruction.
