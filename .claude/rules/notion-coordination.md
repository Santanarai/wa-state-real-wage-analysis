# Notion & Session State Coordination Rule

## Notion
Do NOT update the Notion project hub automatically or as part of task
execution. Notion is the canonical shared record and is updated by the
strategy thread (Claude.ai) after quality review.

Exceptions (user-initiated commands only):
- /checkpoint — mid-session sync to Notion.
- /wrapup — session-end update to Notion (always a NEW row).

Automatic Notion updates during task execution (e.g., after completing a
section) are not permitted without explicit user instruction.

## SESSION_STATE.md
SESSION_STATE.md is a session-end artifact. Update it only during /wrapup
(overwrite, not append). Do not update it during task execution or /checkpoint.
