# Credibility Requirements

This analysis will be used in a bargaining context where the employer side (OFM) has professional consultants and will look for reasons to dismiss the findings. Every design decision below serves one goal: making the analysis impossible to dismiss on methodological grounds.

## Source Citation

- Every data point must trace to a URL. No floating numbers.
- BLS data: cite series ID + URL + date range
- OFM data: cite page URL + access date
- Derived statistics from UW-licensed sources: cite platform name + report/dataset name + date accessed
- If a number comes from a secondary source (news article, union communication), note that and cite the primary source if available

## Reproducibility

- A reader must be able to re-run the notebook and get the same results
- All data retrieval steps must be scripted (API calls or download URLs) or documented with exact retrieval instructions
- Pin library versions in requirements.txt
- If a data source requires authentication (Dewey, Statista), document what was retrieved and when, so the results can be verified by anyone with equivalent access

## No AI-Verified-by-AI Claims

- Do NOT claim outputs were "verified by Wolfram" or "cross-checked by Claude"
- Instead, provide reader-facing verification instructions: "To independently verify this CPI figure, visit data.bls.gov and look up series CUURS49DSA0 for 2020, or enter 'Seattle CPI-U annual average 2020' at wolframalpha.com"
- The credibility comes from the reader being able to check, not from one AI claiming another AI confirmed it

## Compounding Methodology

The WPEA analysis (the existing benchmark) subtracted annual CPI% from annual GWI% without compounding. This is mathematically incorrect and understates erosion in some scenarios, overstates in others. This analysis must:

- Use a chained price index: set a base year (e.g., 2020 = 100), multiply forward by (1 + annual CPI change) each year
- Apply the same chaining to cumulative GWI: base year nominal wage = 100, multiply forward by (1 + GWI%) each year
- Real wage index = (nominal wage index / price index) × 100
- Show the formula explicitly in the notebook, not just the code

## Pre-empting Counterarguments

The employer and right-leaning analysts will make these moves. The analysis must address each:

1. **"Total compensation, not just wages"**: Include at least one section computing total-comp figures using OFM's WSECS total-comp lag (-15.3%) alongside the base-wage analysis
2. **"Step increases mean individual workers got more than GWI"**: Model both a topped-out employee (GWIs only — the worst case) and a progressing employee (GWIs + steps). Present both transparently
3. **"Government workers have job security and pensions"**: Acknowledge these as real forms of compensation without conceding that they offset the wage gap. Note that PERS Plan 2 has no automatic COLA for retirees
4. **"CPI-U overstates inflation for some groups"**: Show results using both CPI-U and CPI-W; note the Seattle-area vs. national distinction

## Wolfram Cross-Checks

Each validation cell that computes a key number should include a Wolfram|Alpha or Wolfram Language cross-check. The purpose is **independent computation** — a second system producing the same result from the same inputs — not "AI verifying AI." The reader can reproduce every check themselves.

### Structure of a cross-check block

Each cross-check is a visible code cell in the notebook (not a hidden step) that prints a formatted block containing:

1. **Query or expression** — The exact Wolfram|Alpha natural-language query or Wolfram Language code
2. **Wolfram result** — The value returned by Wolfram (hardcoded after manual verification)
3. **Our computed value** — The corresponding Python-computed value
4. **Difference** — Absolute and percentage difference
5. **Reader verification URL** — A wolframalpha.com link the reader can click to reproduce

### When to use each tool

- **Wolfram|Alpha** (natural language): For spot-checking individual data points (e.g., `"$100 in 2000 dollars in 2024"`) and cumulative inflation over known periods. Wolfram|Alpha uses national CPI-U; it does not carry Seattle MSA series. Use it for national cross-checks and dollar-equivalent sanity checks.
- **Wolfram Language** (WolframLanguageEvaluator): For independently recomputing derived quantities — chained indices, cumulative rates, year-over-year rates. Feed the same annual averages from our BLS retrieval and confirm the Wolfram Language kernel produces identical results to Python.

### What this does NOT claim

Per the "No AI-Verified-by-AI" rule: these cross-checks demonstrate that **two independent computation engines (Python and Wolfram) agree**, and provide the reader with clickable URLs to reproduce every check. They do not claim that Wolfram "verified" or "approved" the analysis.

## Chart Standards

- Publication-ready: clear labels, titles, axis descriptions
- Source citation on every chart: "Source: BLS Series CUURS49DSA0; OFM GWI History"
- Must work as static images (no interactive-only visualizations) — the brief may be printed
- Consistent color scheme across all charts
- Accessible colors (avoid red-green only distinctions)
