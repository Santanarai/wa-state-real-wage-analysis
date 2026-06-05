# Context Efficiency Rule

When reading files, use targeted reads (specific line ranges) instead of reading entire files.

- For the notebook: read only the cells you need to modify or reference, not the full file. Use line-range views.
- For CSVs: read only the rows needed for validation (head/tail/sample), not the full file.
- Do NOT re-read files that are already in context from the current session.
- When appending cells to the notebook, you do not need to re-read existing sections.
