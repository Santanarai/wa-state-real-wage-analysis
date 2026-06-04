# Data Governance

## Context

The author is a UW Medicine employee with access to UW Libraries research databases (Dewey Data, Statista, FactSet, Social Explorer, etc.) through institutional affiliation. A prior incident involving upload of UW Medicine operational data to a consumer AI tool (Julius AI) with a perpetual license grant in its TOS demonstrated the real consequences of careless data handling. This document exists to prevent a repeat.

## Three Tiers of Data

### Tier 1: Public Data (commit freely)
- BLS economic data (CPI, wages, employment)
- OFM published salary schedules, GWI history, WSECS reports
- DRS/State Actuary published reports
- FRED aggregated economic data
- Census/ACS data
- Any data published by a government agency for public use

**Rule:** Download, commit to repo, redistribute. No restrictions.

### Tier 2: UW-Licensed Research Data (use but do not commit)
- Dewey Data (SafeGraph Spend Patterns, consumer transactions)
- Statista (market/industry statistics)
- Social Explorer (Census visualization/analysis platform)
- FactSet (financial/economic data)
- Any database accessed through UW Libraries subscriptions

**Rule:** Use for analysis. Include derived statistics and findings in the notebook with source attribution. Do NOT commit raw data files, downloaded CSVs, or bulk exports to the repository. The repo should contain your analysis of the data, not the data itself.

**Why:** These platforms are licensed for academic/research use through UW's institutional subscription. Redistributing raw data may violate the data provider's terms of service. Derived statistics with attribution (e.g., "SafeGraph Spend Patterns data shows median healthcare transaction value in King County increased X% from 2020–2025, Source: Dewey Data") are standard academic practice.

**Dewey Data specifically:** Dewey actively supports AI-assisted analysis (they built an MCP server for this purpose). Using their data through Claude Code or other AI tools during analysis is a supported use case. Just don't commit their raw datasets to a public GitHub repo.

### Tier 3: Institutional/Operational Data (never use)
- UW Medicine Workday reports, supply chain data, financial data
- DOH operational data, LIMS data, internal reports
- Any data from employer systems, intranets, or internal databases
- Patient data, PHI, HIPAA-covered information

**Rule:** Absolute prohibition. Do not access, download, reference, or analyze. This analysis uses only economic and wage data from public and licensed academic sources.

## AI Tool Data Handling

- Claude Code (via Claude Pro/API): Does not retain data between sessions. Safe for processing all three tiers during a session, but only Tier 1 outputs should be committed.
- Notion: Use for documentation and narrative only. Do not paste raw Tier 2 data into Notion pages. Charts, methodology notes, and findings are fine.
- GitHub: Public repo. Only Tier 1 data and derived outputs from Tier 2 analysis.
- Dewey MCP: Authenticated data retrieval during Claude Code sessions. Data flows through the session and is not persisted by Claude. Derived outputs only go to the repo.

## When In Doubt

If you're unsure whether a data source can be committed to the repo, it probably can't. Include the finding with a citation instead of the raw data.
