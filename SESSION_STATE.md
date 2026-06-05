# Session State

## Completed (Session 1 — 2026-06-04)

- **Section 1 notebook cells**: 15 cells in `notebooks/real-wage-analysis.ipynb` — CPI data acquisition, annual average computation, chained price index construction (base years 2000 and 2020), inflation rate computation, data export, BLS spot-checks, Wolfram|Alpha cross-checks (3 national CPI), Wolfram Language independent computation (10 chained-index values), WPEA cross-reference, and interpretation. All checks pass.
- **Processed data files**: `data/processed/cpi_annual_averages.csv`, `cpi_chained_indices.csv`, `cpi_inflation_rates.csv`, `data/raw/bls_cpi_raw_records.csv`
- **Repo structure**: Rules extracted to `.claude/rules/` (5 files: data-integrity, data-governance, compounding, editorial-boundaries, wolfram-validation). Known Failure Modes section in CLAUDE.md (4 items). `lessons.md` created.
- **Security fix**: Hardcoded Notion URLs removed from all committed files, moved to `.claude/config.local` (gitignored). History scrubbed with git-filter-repo, force-pushed.
- **Notion hub**: Session log updated with 3 checkpoint entries. Status updated to reflect Section 1 complete.

## Completed (Session 2 — 2026-06-05)

- **Section 2 notebook cells**: 11 cells added to `notebooks/real-wage-analysis.ipynb` — GWI data entry (27 years, 2000–2026, WFSE GG), chained nominal wage index (base years 2000 and 2020), data export, full source verification (all 27 values vs OFM PDF), 5 sanity checks, 4 arithmetic spot-checks, Wolfram Language cross-check (all 54 values match), cross-reference preview (GWI vs CPI side-by-side), and interpretation.
- **Code-reviewer agent**: Audited compounding logic independently — no bugs found.
- **Final execution**: All cells pass, no FAIL lines in output.
- **New data files**: `data/raw/ofm_gwi_raw.csv`, `data/processed/gwi_chained_indices.csv`
- **docs/data-sources.md**: Updated with OFM GWI URLs, PDF revision date, employee group, access date.

## Current State

- Section 1: **DONE and validated** (15 cells, all spot-checks and cross-checks pass)
- Section 2: **DONE and validated** (11 cells, all 27 source values verified, all 54 Wolfram cross-check values match, code-reviewer passed)
- Section 3 (real-wage index + charts): **NOT STARTED**
- No charts built yet (Lauren email blocked on first chart)

### Key Numbers from Section 1
- Seattle CPI-U cumulative inflation 2000–2024: **97.15%**
- Seattle CPI-U cumulative inflation 2020–2024: **25.16%**
- Seattle premium over national: **~15 pp** (97.15% vs 82.17%)

### Key Numbers from Section 2
- Cumulative GWI 2000–2024: **49.30%**
- Cumulative GWI 2020–2024: **10.60%**
- 8 zero-GWI years out of 25 (2002–2004, 2009–2010, 2012, 2014, 2021)
- Only backward year: 2011 (-3% salary reduction, ESSB 5860)
- 2019 dual increase correctly compounded: (1.02)(1.03) = 1.0506
- 2011–2013 roundtrip: 0.97 × 1.03 = 0.9991 (-0.09% artifact)

### The Gap (preview, not yet formalized)
- 2000–2024: GWI +49.30% vs CPI +97.15% → ~48 pp shortfall
- 2020–2024: GWI +10.60% vs CPI +25.16% → ~15 pp shortfall
- These are raw percentage gaps; Section 3 will compute the proper real-wage index (wage_index / price_index × 100)

## Open Issues

- BLS-published annual averages differ from our computed averages by up to 0.3 CPI points (rounding in bimonthly-to-annual averaging). Within tolerance, documented, all spot-checks pass.
- Wolfram|Alpha does not carry Seattle MSA CPI series. National cross-checks only. Seattle validated via BLS direct spot-checks + Wolfram Language independent computation.
- `lessons.md` is empty — no errors have occurred yet to document.
- 2025–2026 GWI values are legislatively approved but forward-looking (no matching CPI data yet). CPI comparison limited to 2000–2024.

## Next Steps

1. **Start Section 3: Real-Wage Index**. Compute real_wage_index = (wage_index / price_index) × 100 for both base years. Produce publication-ready charts with source citations. Follow five-part cell pattern.
2. **After first chart**: Send Lauren Boyan email with repo link.
3. **Later sections**: WSECS market-lag integration (-18.2% base / -15.3% total comp), total-compensation counter, progressing-employee model.

## Do NOT Re-read

- `docs/data-sources.md` — already incorporated into notebook
- `docs/documentation-pattern.md` — five-part pattern is established
- `docs/credibility-requirements.md` — standards are in place, including Wolfram section
- `docs/data-governance.md` — rules extracted to `.claude/rules/`
- The Notion project hub methodology section — all decisions are settled and reflected in the repo
- OFM GWI PDF — fully transcribed and validated in Section 2
