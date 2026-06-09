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
  (URL in `.claude/config.local` as `NOTION_HUB_URL`; also contains `NOTION_BEST_PRACTICES_URL`)
  Contains methodology decisions, counterargument prep, key reference numbers,
  and session log. Read this page at the start of every session for strategic
  context that isn't in the repo files.

Read the relevant doc before starting work in that area. Do not proceed from memory alone.

<rules>
## Critical Rules

Immutable project rules live in `.claude/rules/`. These override any other guidance.

- `data-integrity.md` — No synthetic data, no unsourced numbers
- `data-governance.md` — What can and cannot be committed
- `compounding.md` — Chained indexing only, never simple subtraction
- `editorial-boundaries.md` — Present data, don't make demands or claim credentials
- `wolfram-validation.md` — Cross-check key values via Wolfram MCP at every validation point
- `context-efficiency.md` — Targeted file reads; no full-file dumps
- `notion-coordination.md` — Notion updates are manual only (/checkpoint, /wrapup); SESSION_STATE.md is session-end only
</rules>

## Known Failure Modes

1. **The Confident Declaration** — Never claim verification without actually running code and checking output against an external reference. "The spot-checks pass" means nothing if you didn't execute the cell.
2. **The Parse Check** — Code running without errors does not mean the logic is correct. The compounding calculation must be validated against a manual example every time it is modified.
3. **The 7% Read** — When processing multi-year BLS datasets, confirm you loaded the full date range, not a subset. Check the first and last year in the dataframe before proceeding.
4. **The Second Opinion** — After completing any critical calculation (compounding, real-wage index, cumulative GWI), invoke the code-reviewer agent to audit the logic independently before marking the section as validated.

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

### Session Commands

Available via `.claude/skills/` (invoked as `/rampup`, `/checkpoint`, etc.):

- `rampup/SKILL.md` — Session startup checklist (read docs, orient)
- `checkpoint/SKILL.md` — Mid-session sync to Notion project hub
- `sync/SKILL.md` — Read Notion project hub for updates from the strategy thread
- `wrapup/SKILL.md` — Session-end protocol (SESSION_STATE.md, Notion, commit)

## Technical Stack
- Python 3.x, pandas, matplotlib/seaborn
- Jupyter notebook (.ipynb) as primary artifact
- All dependencies in requirements.txt
- Charts must be publication-ready with source citations on every figure
- No API keys or credentials in the repo
- Use `ultrathink` for critical calculations (compounding, real-wage index). It is parsed deterministically by the harness; "think deeply" is not.

### Local Setup
```
python -m venv .venv
.venv\Scripts\activate        # Windows
source .venv/bin/activate     # macOS/Linux
pip install -r requirements.txt
```
Optional — register the Jupyter kernel for local notebook use:
```
python -m ipykernel install --user --name=wa-real-wage --display-name "WA Real-Wage Analysis"
```
Claude Code sessions should run `pip install -r requirements.txt` at session start to match the pinned environment.

## Repository Structure
```
wa-state-real-wage-analysis/
├── .claude/
│   ├── config.local              # Notion URLs (not committed)
│   ├── skills/                   # Session lifecycle skills
│   │   ├── checkpoint/SKILL.md
│   │   ├── rampup/SKILL.md
│   │   ├── sync/SKILL.md
│   │   └── wrapup/SKILL.md
│   └── rules/                   # Immutable project rules
│       ├── compounding.md
│       ├── context-efficiency.md
│       ├── data-governance.md
│       ├── data-integrity.md
│       ├── editorial-boundaries.md
│       ├── notion-coordination.md
│       └── wolfram-validation.md
├── .gitattributes
├── .gitignore
├── CLAUDE.md
├── README.md
├── requirements.txt
├── SESSION_STATE.md
├── SESSION_HISTORY.md
├── lessons.md                    # Recurring mistake log
├── todo/
│   └── observations.md
├── docs/
│   ├── data-sources.md
│   ├── documentation-pattern.md
│   ├── credibility-requirements.md
│   └── data-governance.md
├── data/
│   ├── raw/
│   │   ├── bls_cpi_raw_records.csv
│   │   └── ofm_gwi_raw.csv
│   └── processed/
│       ├── cpi_annual_averages.csv
│       ├── cpi_chained_indices.csv
│       ├── cpi_inflation_rates.csv
│       ├── gwi_chained_indices.csv
│       └── real_wage_index.csv
├── notebooks/
│   └── real-wage-analysis.ipynb
└── output/
    ├── charts/                   # Publication-ready figures
    │   ├── fig1_wage_vs_price_divergence.png
    │   ├── fig2_real_wage_index_2000.png
    │   ├── fig3_annual_real_wage_change.png
    │   └── fig4_recent_cycle_2020.png
    └── brief/
```

## Benchmark
WPEA produced a 25-year table showing "21.05% total lost purchasing power" using simple subtraction (GWI% minus CPI%, no compounding). This analysis must: use proper chained indexing, model both topped-out and progressing employees, integrate OFM's WSECS market-lag data (-18.2% base / -15.3% total comp), address the total-compensation counter, and be fully reproducible.
