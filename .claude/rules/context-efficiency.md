# Context Efficiency

When reading files, use targeted reads (specific line ranges) instead of reading
entire files. Do not re-read files that are already in context from the current
session.

## Why this rule exists

Claude Code's default behavior is to read entire files when referencing them. For
small source files this is fine. For large structured files — notebooks, CSVs, data
files, configs, long markdown documents — full reads consume disproportionate context.
Before this rule was introduced, file reads were consuming 50–65% of available context
in a typical data analysis session, leaving too little room for actual reasoning.

## What violation looks like

Claude reads an entire 2,000-line notebook to modify 3 cells. Claude re-reads a file
it already read 5 turns ago. Claude reads a full CSV (thousands of rows) to verify a
single summary statistic. Claude reads an entire CLAUDE.md or docs file to check one
section. The symptom is /context showing file-read content dominating the window, or
compaction triggering earlier than expected.

## Failure pattern: The 7% Read (inverted)

The named failure mode "The 7% Read" describes Claude reading only a portion of a file
and proceeding with full confidence. This rule addresses the opposite failure: reading
100% of a file when 7% would suffice. Both waste resources — one wastes accuracy, the
other wastes context.

## Specific guidance

- For large structured files (notebooks, configs, data files): read only the sections
  you need to modify or reference. Use line-range views.
- For CSVs and tabular data: read only head/tail/sample rows needed for validation,
  not the full file.
- When appending to or extending a file, you do not need to re-read existing sections.
- After compaction, verify that files you need are still in context before re-reading
  them — they may have survived the summary.
