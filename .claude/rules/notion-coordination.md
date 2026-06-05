# Notion Coordination Rule

Do NOT update the Notion project hub automatically or as part of task execution.
Notion is the canonical shared record and is updated by the strategy thread
(Claude.ai) after quality review.

Exception: the /checkpoint command is user-initiated and may update Notion
when explicitly invoked.

SESSION_STATE.md is your working memory — update it at every checkpoint and
before any task that might trigger auto-compaction. This is critical for
surviving context loss.

Division of responsibility:
- Claude Code → SESSION_STATE.md (update freely)
- Claude Code /checkpoint → Notion (user-initiated, OK)
- Claude.ai strategy thread → Notion (primary, after QC)
