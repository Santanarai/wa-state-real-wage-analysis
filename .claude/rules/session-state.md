# Session State Discipline

SESSION_STATE.md is a session-end artifact, not a live working document. Update it
only at session end (overwrite, not append). Do not update it during task execution.

## Why this rule exists

Claude Code's default behavior is to treat any state-tracking file as something it
should keep current. Without this rule, Claude will update SESSION_STATE.md after
completing each subtask — adding completed items, adjusting next steps, rewriting
the current state. This feels helpful but creates several problems.

## What violation looks like

Mid-task, Claude writes to SESSION_STATE.md without being asked. The symptom is
commits or file changes to SESSION_STATE.md that appear between session start and
session end, or SESSION_STATE.md edits interleaved with task work. Claude may frame
this as "keeping the session state current" or "updating progress."

## Failure pattern: Session State Drift

When SESSION_STATE.md is updated mid-session, it conflates partial progress with
session-end state. If the session is interrupted (context limit, crash, user
abandons the thread), the file contains a half-finished snapshot that the next
session reads as authoritative. Completed items may be listed that were never
verified. Next steps may reflect a mid-task plan that was later abandoned.

The session-end protocol exists specifically to produce a clean, verified,
end-of-session snapshot. Mid-session updates bypass that verification step. They
also consume context on file writes that add no value — the session is still in
progress, so the file will be overwritten at session end anyway.

If you need to persist progress mid-session for crash protection, use a commit
with a descriptive message. Do not update SESSION_STATE.md directly.

## Session start protocol

At the beginning of every session, before writing any code:

1. Read `CLAUDE.md` and `SESSION_STATE.md`.
2. Report back with: what was completed last session, what the next planned step
   is, and any open issues or blockers from SESSION_STATE.md.

Do not start writing code until this orientation is complete and confirmed.

## Session end protocol

When the user asks to wrap up or end the session:

1. **Archive session state.** Append the full contents of `SESSION_STATE.md` to
   `SESSION_HISTORY.md`, preceded by a separator line:
   `--- Session [N] closed [date] ---`
2. **Overwrite `SESSION_STATE.md`** with the current state:
   - **Completed:** What was accomplished this session
   - **Current state:** Which notebook cells are done, validated, or pending
   - **Open issues:** Anything unresolved, flagged, or needing investigation
   - **Next steps:** Exactly what the next session should start with
   - **Do NOT re-read:** Files already processed that don't need revisiting
3. **Commit all uncommitted changes** with a descriptive message summarizing the
   session's work.
4. **Report** a brief summary of what was accomplished and what the next session
   should focus on.
