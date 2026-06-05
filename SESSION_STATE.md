# Session State

## Completed (Session 1 — 2026-06-04)

- **Section 1 notebook cells**: 15 cells in `notebooks/real-wage-analysis.ipynb` — CPI data acquisition, annual average computation, chained price index construction (base years 2000 and 2020), inflation rate computation, data export, BLS spot-checks, Wolfram|Alpha cross-checks (3 national CPI), Wolfram Language independent computation (10 chained-index values), WPEA cross-reference, and interpretation. All checks pass.
- **Processed data files**: `data/processed/cpi_annual_averages.csv`, `cpi_chained_indices.csv`, `cpi_inflation_rates.csv`, `data/raw/bls_cpi_raw_records.csv`
- **Repo structure**: Rules extracted to `.claude/rules/` (5 files: data-integrity, data-governance, compounding, editorial-boundaries, wolfram-validation). Known Failure Modes section in CLAUDE.md (4 items). `lessons.md` created.
- **Security fix**: Hardcoded Notion URLs removed from all committed files, moved to `.claude/config.local` (gitignored). History scrubbed with git-filter-repo, force-pushed.

## Completed (Session 2 — 2026-06-05)

- **Section 2 notebook cells**: 11 cells — GWI data entry (27 years, 2000–2026, WFSE GG), chained nominal wage index (base years 2000 and 2020), data export, full source verification (all 27 values vs OFM PDF), 5 sanity checks, 4 arithmetic spot-checks, Wolfram Language cross-check (all 54 values match), cross-reference preview, and interpretation.
- **Section 3 notebook cells**: 13 cells — real-wage index computation, data export, chart configuration, 4 publication-ready charts, validation (7 checks + 50-value Wolfram cross-check), and interpretation.
- **Kernel fix**: Notebook kernelspec changed from `python3` (system Python) to `wa-real-wage` (venv with matplotlib/seaborn). This was the blocker preventing chart generation.
- **Charts generated** (all saved to `output/charts/`):
  - `fig1_wage_vs_price_divergence.png` — Dual-line wages (149.3) vs prices (197.2), shaded gap
  - `fig2_real_wage_index_2000.png` — Real purchasing power decline, 100 → 75.7 (24.3% loss)
  - `fig3_annual_real_wage_change.png` — YoY bar chart, 2001–2024, color-coded by sign
  - `fig4_recent_cycle_2020.png` — Recent cycle 2020–2024, 11.6% loss in 4 years
- **Validation**: All 7 checks PASS, all 50 Wolfram cross-check values MATCH (max diff 0.002)
- **CPI-W sensitivity**: 24.9% loss (CPI-W) vs 24.3% (CPI-U) — result robust to CPI variant
- **New data files**: `data/raw/ofm_gwi_raw.csv`, `data/processed/gwi_chained_indices.csv`, `data/processed/real_wage_index.csv` (50 rows, 25 years × 2 bases)
- **Code-reviewer agent**: Audited Section 2 compounding logic independently — no bugs found.
- **Rules added/updated**: `notion-coordination.md` (3 iterations → canonical form with SESSION_STATE.md clause), `context-efficiency.md`
- **Housekeeping**: `.gitignore` updated for nbconvert artifacts, cleaned up `real-wage-analysis-executed.ipynb`
- **docs/data-sources.md**: Updated with OFM GWI URLs, PDF revision date, employee group, access date.

## Current State

- Section 1: **DONE and validated** (15 cells, all spot-checks and cross-checks pass)
- Section 2: **DONE and validated** (11 cells, all 27 source values verified, all 54 Wolfram cross-check values match, code-reviewer passed)
- Section 3: **DONE and validated** (13 cells, all 7 checks pass, all 50 Wolfram values match, 4 charts generated)
- **Total notebook**: 39 cells (15 markdown + 24 code)

### Key Numbers

| Metric | Base 2000 | Base 2020 |
|--------|-----------|-----------|
| Cumulative CPI (Seattle CPI-U) | +97.15% | +25.16% |
| Cumulative GWI (WFSE GG) | +49.30% | +10.60% |
| Real-wage index (endpoint) | 75.7 | 88.4 |
| Purchasing power loss | **24.3%** | **11.6%** |
| CPI-W sensitivity | 24.9% loss | 11.3% loss |

- WPEA's simple-subtraction estimate: ~21% — our compounded figure (24.3%) is larger, as expected
- Worst single year: 2011 (-5.6% real change: -3% salary cut + CPI rise)
- Worst recent year: 2021 (-4.8% real change: 0% GWI + 5% CPI)

## Open Issues

- BLS API daily rate limit prevents re-execution of Section 1 fetch cell. All downstream cells work from saved CSVs. Re-execute from a fresh IP or on the next calendar day.
- 2025–2026 GWI values are legislatively approved but forward-looking (no matching CPI data yet). CPI comparison limited to 2000–2024.
- Notion outreach status still says "Waiting on first chart build" — charts are now done. Lauren email is actionable.

## Next Steps

1. **Section 4: WSECS Market-Lag Integration** — Incorporate OFM's WSECS data (-18.2% base salary lag, -15.3% total compensation lag from June 2024 final report) as independent corroboration. Read `docs/data-sources.md` for the WSECS source details.
2. **Section 5: Total-Compensation Counter** — Address the "but benefits" argument with data.
3. **Section 6: Progressing-Employee Model** — Model employees who are NOT topped out (step progressions + GWI).
4. **Lauren Boyan email** — Charts are built; outreach is unblocked.
5. **Push `575c59c`** — Latest coordination rule commit needs to be pushed to origin.

## Do NOT Re-read

- `docs/data-sources.md` — already incorporated into notebook (re-read only for WSECS source URLs when starting Section 4)
- `docs/documentation-pattern.md` — five-part pattern is established
- `docs/credibility-requirements.md` — standards are in place, including Wolfram section
- `docs/data-governance.md` — rules extracted to `.claude/rules/`
- The Notion project hub methodology section — all decisions are settled and reflected in the repo
- OFM GWI PDF — fully transcribed and validated in Section 2
