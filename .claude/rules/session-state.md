# Session State Discipline

SESSION_STATE.md is a session-end artifact, not a live working document. Update it
only during /wrapup (overwrite, not append). Do not update it during task execution.

## Why this rule exists

Claude Code's default behavior is to treat any state-tracking file as something it
should keep current. Without this rule, Claude will update SESSION_STATE.md after
completing each subtask — adding completed items, adjusting next steps, rewriting
the current state. This feels helpful but creates several problems.

## What violation looks like

Mid-task, Claude writes to SESSION_STATE.md without being asked. The symptom is
commits or file changes to SESSION_STATE.md that appear between /rampup and /wrapup,
or SESSION_STATE.md edits interleaved with task work. Claude may frame this as
"keeping the session state current" or "updating progress."

## Failure pattern: Session State Drift

When SESSION_STATE.md is updated mid-session, it conflates partial progress with
session-end state. If the session is interrupted (context limit, crash, user
abandons the thread), the file contains a half-finished snapshot that the next
/rampup reads as authoritative. Completed items may be listed that were never
verified. Next steps may reflect a mid-task plan that was later abandoned.

The /wrapup command exists specifically to produce a clean, verified, end-of-session
snapshot. Mid-session updates bypass that verification step. They also consume
context on file writes that add no value — the session is still in progress, so
the file will be overwritten at /wrapup anyway.

If you need to persist progress mid-session for crash protection, use a commit
with a descriptive message — or /checkpoint if the project has one configured.
Do not update SESSION_STATE.md directly.
