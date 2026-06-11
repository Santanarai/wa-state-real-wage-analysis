# Session State

## Completed (Session 3 — 2026-06-11)

- **WSECS PDFs committed**: `2020 WaSurV Final Reprt1.pdf`, `2022_WSECS_State_Report_04-2022.pdf`, `UPDATED_FINAL REPORT_2024 WSECS Report June 2024.pdf`, `Revised 2024_WA_SECS_Survey_Data_Tables_06-2024.xlsm`
- **Benchmark job count fix** (commit `126444d`): 197 → 198 (181 reportable) in notebook Section 3 interpretation and `docs/data-sources.md`
- **Section 4: WSECS Market-Lag Integration** (commit `fe51726`): 10 new cells
  - Headline figures from all 3 WSECS reports (original report values, not retrospective)
  - 16 market segments extracted from .xlsm data tables, cross-checked against PDF
  - `fig5_wsecs_market_lag_trend.png` — grouped bar chart with total comp widening annotation
  - Source verification: 9 figures verified with PDF page numbers and verbatim quotes
  - 2022 retrospective discrepancy documented (2024 report says -18.7%/-15.9%; 2022 report says -19.1%/-16.3%)
  - Framed as convergent evidence, not additive
- **openpyxl** added to requirements.txt and venv

## Current State

- Section 1: **DONE and validated** (15 cells)
- Section 2: **DONE and validated** (11 cells)
- Section 3: **DONE and validated** (13 cells, 4 charts)
- Section 4: **DONE and validated** (10 cells, 1 chart)
- **Total notebook**: 49 cells

### Key Numbers

| Metric | Base 2000 | Base 2020 |
|--------|-----------|-----------|
| Cumulative CPI (Seattle CPI-U) | +97.15% | +25.16% |
| Cumulative GWI (WFSE GG) | +49.30% | +10.60% |
| Real-wage index (endpoint) | 75.7 | 88.4 |
| Purchasing power loss | **24.3%** | **11.6%** |

### WSECS Market Lag (Section 4)

| Year | Base Salary | Total Comp | Benchmark Jobs |
|------|-------------|------------|----------------|
| 2020 | -17.7% | -12.3% | 185 |
| 2022 | -19.1% | -16.3% | 197 |
| 2024 | -18.2% | -15.3% | 198 (181 reportable) |

Total comp gap widened 3.0 pp (2020→2024). All 16 market segments negative (-8.7% to -31.3%).

## Open Issues

- BLS API daily rate limit prevents re-execution of Section 1 fetch cell. Downstream cells work from saved CSVs.
- 2025–2026 GWI values are legislatively approved but forward-looking (no matching CPI data yet).

## Next Steps

1. **Section 5: Total-Compensation Counter** — Address the "but benefits" argument with data.
2. **Section 6: Progressing-Employee Model** — Model employees who are NOT topped out (step progressions + GWI).
3. **Lauren Boyan email** — Charts are built; outreach is unblocked. Draft prepared but not yet sent.

## Do NOT Re-read

- `docs/data-sources.md` — fully updated through Section 4
- `docs/documentation-pattern.md` — five-part pattern is established
- `docs/credibility-requirements.md` — standards are in place
- `docs/data-governance.md` — rules extracted to `.claude/rules/`
- OFM GWI PDF — fully transcribed and validated in Section 2
- WSECS PDFs — all figures verified and cited with page numbers in Section 4
