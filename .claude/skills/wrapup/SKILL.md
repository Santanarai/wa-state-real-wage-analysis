---
name: wrapup
description: Session end — archives state, commits all work
disable-model-invocation: true
---
Session is ending. Complete the following before stopping:

1. **Archive session state.** Append the full contents of `SESSION_STATE.md` to `SESSION_HISTORY.md`, preceded by a line: `--- Session [N] closed [date] ---`. This preserves the detailed session state before it gets overwritten.

2. **Update `SESSION_STATE.md`** in the repo root. Overwrite (do not append) with:
   - **Completed:** What was accomplished this session
   - **Current state:** Which notebook cells are done, validated, or pending
   - **Open issues:** Anything unresolved, flagged, or needing investigation
   - **Next steps:** Exactly what the next session should start with
   - **Do NOT re-read:** Files already processed that don't need revisiting next session

3. **Commit all uncommitted changes** to the repo with a descriptive commit message summarizing the session's work.

4. **Report** a brief summary of what was accomplished and what the next session should focus on.
