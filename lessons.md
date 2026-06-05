# Lessons Learned

When a mistake is made during a session and corrected, add an entry here documenting the failure pattern and the fix. This file exists so the same mistake isn't repeated across sessions.

## Format

Each entry should include:
- **Date**: When the mistake occurred
- **What went wrong**: The specific error
- **Why**: Root cause — what assumption or shortcut led to it
- **Rule**: The principle that prevents recurrence

---

### 2026-06-04 — Slash command tried to invoke /compact

- **What went wrong**: `/smartcompact` attempted to invoke `/compact` from within a slash command. Claude Code generated "/compact" as text output instead of executing it as a system command. Context increased from 88% to 94% because the verbose retain/discard analysis was added to context but compaction never actually ran.
- **Why**: Slash commands are prompt injections — they tell Claude Code what to do in natural language, but cannot programmatically invoke built-in system commands like `/compact`, `/clear`, or `/model`.
- **Rule**: Never design a slash command that depends on invoking another command. Use two-step workflows: the command generates output, the user executes the system command manually with that output. Keep command output concise since it adds to context before the user can act on it.
