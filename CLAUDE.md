# CLAUDE.md — WA State Real-Wage Erosion Analysis

## Project

Reproducible analysis of real-wage erosion for WA state classified employees, built for WFSE (AFSCME Council 28) bargaining teams heading into 2027–2029 negotiations. The deliverable is a Jupyter notebook that serves as both the analysis and its own proof of correctness.

**Author:** Gabe Santana — classified state employee (DOH + UW Medicine), WFSE member in GG and UW units. Not a data scientist. This analysis is AI-assisted: Claude Code generates the Python code, Gabe validates outputs and makes editorial decisions.

**Core question:** How much real purchasing power have WA classified employees gained or lost since 2000/2020, measured by cumulative GWIs indexed against CPI with proper compounding?

## Detailed Documentation

- `docs/data-sources.md` — All data sources, series IDs, URLs, and access rules
- `docs/documentation-pattern.md` — Five-part notebook cell structure
- `docs/credibility-requirements.md` — Source citation, verification, and reproducibility standards
- `docs/data-governance.md` — What goes in the repo vs. what doesn't, UW license constraints
- **Notion Project Hub** — `WA Real-Wage Erosion Analysis — Project Hub`
  (NOTION_HUB_URL_REDACTED)
  Contains methodology decisions, counterargument prep, key reference numbers,
  and session log. Read this page at the start of every session for strategic
  context that isn't in the repo files.

Read the relevant doc before starting work in that area. Do not proceed from memory alone.

<rules>
## Critical Rules — Parse These as Sacred

### Data Integrity
- NEVER generate synthetic or estimated data. If a data point is missing, say so.
- NEVER hardcode a number without documenting its source URL and series ID.
- Every BLS figure must include the series ID (e.g., CUURS49DSA0) and a verification URL.
- Every OFM figure must link to the specific OFM page with access date noted.

### Data Governance
- Public data (BLS, OFM, DRS): commit freely.
- UW-licensed data (Dewey, Statista, Social Explorer): use for analysis but NEVER commit raw data files. Include only derived statistics with source attribution.
- No UW Medicine operational data. No PHI. No Workday data. No institutional sources.

### Compounding
- Use chained indexing for real-wage calculations. Do NOT subtract annual CPI from annual GWI percentages — that is the methodological error this analysis exists to correct.

### Editorial Boundaries
- Do not make claims about what the bargaining team should demand. Present data; strategic interpretation belongs to the union.
- Do not claim analytical skills or credentials on behalf of the author.
</rules>

## Workflow Protocol

<plan_first>
### Before Writing Any Code Cell
1. State in a markdown cell what question this section answers
2. Identify the exact data needed (series IDs, URLs, date ranges)
3. Describe the calculation to be performed and expected output shape
4. Only then write and execute the code

Do not combine planning and execution in the same step. Plan first, get confirmation or self-review, then execute.
</plan_first>

<session_protocol>
### Session Handoff
Before ending any session, write a `SESSION_STATE.md` file to the repo root containing:
1. **Completed**: What was accomplished this session
2. **Current state**: Where the analysis stands (which cells are done, validated, or pending)
3. **Open issues**: Anything unresolved, flagged, or needing investigation
4. **Next steps**: Exactly what the next session should start with
5. **Do NOT re-read**: Files already processed that don't need revisiting

Update this file, do not append — it should always reflect the current state.
</session_protocol>

## Technical Stack
- Python 3.x, pandas, matplotlib/seaborn
- Jupyter notebook (.ipynb) as primary artifact
- All dependencies in requirements.txt
- Charts must be publication-ready with source citations on every figure
- No API keys or credentials in the repo

## Repository Structure
```
wa-state-real-wage-analysis/
├── CLAUDE.md
├── README.md
├── requirements.txt
├── SESSION_STATE.md
├── docs/
│   ├── data-sources.md
│   ├── documentation-pattern.md
│   ├── credibility-requirements.md
│   └── data-governance.md
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
│   └── real-wage-analysis.ipynb
└── output/
    ├── charts/
    └── brief/
```

## Benchmark
WPEA produced a 25-year table showing "21.05% total lost purchasing power" using simple subtraction (GWI% minus CPI%, no compounding). This analysis must: use proper chained indexing, model both topped-out and progressing employees, integrate OFM's WSECS market-lag data (-18.2% base / -15.3% total comp), address the total-compensation counter, and be fully reproducible.
