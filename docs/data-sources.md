# Data Sources

## Primary Sources (Public, No Restrictions)

### BLS Consumer Price Index
| Series | Description | Use |
|--------|-------------|-----|
| CUURS49DSA0 | CPI-U, Seattle-Tacoma-Bellevue, All Items | Primary regional inflation measure |
| CUUR0000SA0 | CPI-U, US City Average, All Items | National comparison |
| CWURS49DSA0 | CPI-W, Seattle-Tacoma-Bellevue, All Items | Wage-earner-weighted alternative |
| CWUR0000SA0 | CPI-W, US City Average, All Items | National CPI-W comparison |

- Pull from FRED (fred.stlouisfed.org) or BLS directly (data.bls.gov)
- Use annual averages unless a specific month-to-month comparison is needed
- Always cite the exact series ID in code comments and notebook markdown

### OFM Compensation Data
- **General Wage Increase history**: Published on OFM's compensation/collective bargaining page. Document exact URL and access date.
- **Salary schedules**: Published pay ranges by classification and range/step. Document exact URL.
- **WSECS (WA State Employee Compensation Survey)**: June 2024 final report. Key findings: -18.2% base salary lag, -15.3% total compensation lag. Cite the FINAL report, not the May 2024 draft (which showed -16.5%).

### WA DRS / Office of the State Actuary
- PERS Plan 2 data for pension/COLA context if the analysis extends there
- Actuarial Valuation Reports published annually

### Other Public Sources
- US Bureau of Economic Analysis (bea.gov) — regional price parities, personal income
- FRED (fred.stlouisfed.org) — aggregator for BLS, Fed, and other federal economic data

## Supplementary Sources (UW-Licensed)

These require UW NetID access. Use for analysis but do NOT commit raw data to the repo.

### Dewey Data (SafeGraph Spend Patterns)
- Healthcare transaction data for Seattle metro area
- Use to validate or challenge CPI-Medical-Care sub-index
- Include only derived statistics (e.g., "median healthcare spend per visit in King County rose X% from 2020-2025") with source attribution

### Statista
- Private-sector wage benchmarks by industry and region
- Market/industry statistics for contextual comparisons
- Same rule: derived statistics only, cite "Source: Statista, [specific report name]"

### Social Explorer
- Census demographic data for geographic analysis
- State worker residential distribution vs. cost-of-living variation by county
- Derived outputs only

## Verification Instructions for Readers

For any data point in the analysis, readers can independently verify by:
1. BLS data: Visit data.bls.gov, enter the series ID, select the date range
2. OFM data: Visit the URL cited in the notebook
3. Wolfram|Alpha: Enter natural language queries like "Seattle CPI-U annual average 2020" at wolframalpha.com
4. FRED: Visit fred.stlouisfed.org and search by series ID
