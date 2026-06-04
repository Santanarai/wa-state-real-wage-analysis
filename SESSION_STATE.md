# Session State

## Completed (Session 1 — 2026-06-04)

- **Section 1 notebook cells**: 15 cells in `notebooks/real-wage-analysis.ipynb` — CPI data acquisition, annual average computation, chained price index construction (base years 2000 and 2020), inflation rate computation, data export, BLS spot-checks, Wolfram|Alpha cross-checks (3 national CPI), Wolfram Language independent computation (10 chained-index values), WPEA cross-reference, and interpretation. All checks pass.
- **Processed data files**: `data/processed/cpi_annual_averages.csv`, `cpi_chained_indices.csv`, `cpi_inflation_rates.csv`, `data/raw/bls_cpi_raw_records.csv`
- **Repo structure**: Rules extracted to `.claude/rules/` (5 files: data-integrity, data-governance, compounding, editorial-boundaries, wolfram-validation). Known Failure Modes section in CLAUDE.md (4 items). `lessons.md` created.
- **Security fix**: Hardcoded Notion URLs removed from all committed files, moved to `.claude/config.local` (gitignored). History scrubbed with git-filter-repo, force-pushed.
- **Notion hub**: Session log updated with 3 checkpoint entries. Status updated to reflect Section 1 complete.

## Current State

- Section 1: **DONE and validated** (15 cells, all spot-checks and cross-checks pass)
- Section 2 (GWI history): **NOT STARTED**
- No charts built yet (Lauren email blocked on first chart)

### Key Numbers from Section 1
- Seattle CPI-U cumulative inflation 2000–2024: **97.2%**
- Seattle CPI-U cumulative inflation 2020–2024: **25.2%**
- Seattle premium over national: **~15 pp** (97.2% vs 82.2%)
- National 2000–2024: 82.2% (matches Wolfram|Alpha's 82.17%)

## Open Issues

- BLS-published annual averages differ from our computed averages by up to 0.3 CPI points (rounding in bimonthly-to-annual averaging). Within tolerance, documented, all spot-checks pass.
- Wolfram|Alpha does not carry Seattle MSA CPI series. National cross-checks only. Seattle validated via BLS direct spot-checks + Wolfram Language independent computation.
- `lessons.md` is empty — no errors have occurred yet to document.

## Next Steps

1. **Start Section 2: GWI History**. Pull WA state General Wage Increase history from OFM. Need exact URL, access date, and GWI percentages by year (2000–2024). Build cumulative GWI index using chained compounding (same method as CPI). Follow five-part cell pattern.
2. **After Section 2**: Build the real-wage index (Section 3) — nominal wage index / price index × 100.
3. **After first chart**: Send Lauren Boyan email with repo link.

## Do NOT Re-read

- `docs/data-sources.md` — already incorporated into notebook
- `docs/documentation-pattern.md` — five-part pattern is established
- `docs/credibility-requirements.md` — standards are in place, including Wolfram section
- `docs/data-governance.md` — rules extracted to `.claude/rules/`
- The Notion project hub methodology section — all decisions are settled and reflected in the repo
