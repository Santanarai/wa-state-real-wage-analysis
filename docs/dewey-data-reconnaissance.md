# Dewey Data Reconnaissance — Supplementary Dataset Assessment

**Date:** 2026-06-10
**Purpose:** Assess Dewey Data platform datasets for supplementary analysis supporting the WA State Real-Wage Erosion project.
**Data governance:** All Dewey datasets are Tier 2 under project rules — use for analysis only; commit derived statistics with source attribution, never raw data.

---

## Access Summary

All datasets below are accessible via UW institutional subscription (download-level access) unless noted otherwise. Personal account access is view-only (level 11) for most datasets.

---

## 1. BrightQuery — Occupational Wage Data [HIGHEST PRIORITY]

**Slug:** `occupational-wage-data`
**DOI:** N/A (no DOI listed)
**Partner:** BrightQuery

This dataset was not in the original search brief but is the single most valuable find for this project.

### Coverage
- **Geographic:** US national, state-level, MSA-level, and non-metro areas. Washington State and Seattle-Tacoma MSA are directly available via `BQ_AREA_TYPE` (2=State, 4=MSA) and `BQ_ADDRESS_STATE`.
- **Temporal:** 2010 to present (time series). Misses 2000–2009.
- **Rows:** 25.5M | **Format:** CSV | **Refresh:** Monthly

### Key Variables
- **800+ occupations** with SOC codes (`BQ_OCC_CD`) and descriptions
- **Wage percentiles:** 10th, 25th, median, 75th, 90th — both hourly and annual
- **Mean wages:** hourly and annual, with percent relative standard error
- **Employment estimates** and location quotients
- **NAICS industry breakdown** at multiple levels
- **Ownership type (`BQ_OCC_OWNERSHIP_CD`):** 2=State Government, 3=Local Government, 5=Private, and combinations

### Why This Matters
The ownership-type field is the critical differentiator. This dataset enables direct comparisons like:

> "In the Seattle MSA, state government Administrative Assistants earned a median of $X in 2010 and $Y in 2024, while private-sector Administrative Assistants went from $A to $B."

This is precisely the OFM/WSECS market-lag argument quantified at the occupation level.

### Limitations
- Derived from BLS Occupational Employment and Wage Statistics (OEWS) — the underlying data is public. BrightQuery's value-add is normalization, time-series formatting, and convenience.
- Starts 2010, so cannot cover the full 2000–2024 window for the core analysis.
- OEWS is a survey of establishments; self-employed workers are excluded.
- SOC codes don't map 1:1 to WA state classified job titles — crosswalking is needed.

### Assessment
**Worth a dedicated supplementary section.** Enables the strongest possible response to the "total compensation" counter-argument by showing occupation-level public/private wage divergence in the Seattle labor market over 14 years. Could be Section 6 or 7 of the notebook.

---

## 2. WageScape — Job Postings with Salary

**Slug:** `job-postings-with-salary`
**DOI:** https://doi.org/10.82551/TSAG-NZ08
**Citation:** WageScape. (2022). Job Postings with Salary [Dataset]. Dewey Data.
**Partner:** WageScape (powered by Revelio Labs)

### Coverage
- **Geographic:** US-wide. Fields: `STATE` (WA), `CITY`, `ZIP`, `COUNTY`, `REGION_STATE` (MSA-level, e.g., "Seattle-Tacoma-Bellevue| WA MSA"). Washington is confirmed in categorical values.
- **Temporal:** POST_DATE from 2016-04-25 to 2025-03-01 (Indeed from 2016; LinkedIn from 2021). **1-year lag** on updates.
- **Rows:** 323.6M | **Format:** Parquet | **Refresh:** Monthly

### Key Variables
- `SALARY` — expected annual salary (70% populated; 30% null)
- `SALARY_MODELED` — Revelio Labs predicted salary when missing (17% of rows)
- `COMPANY`, `NAICS_PRIMARY`, `SIC_PRIMARY` — employer identification
- `ROLE_PRIMARY` — **entirely null (0 populated rows)**; must use supplementary Role Mapping dataset
- Supplementary datasets: Titles, Role Mapping (O*NET codes, job families), Time Log, Tags

### Limitations
- **No public/private sector flag.** Must infer from company name (e.g., "State of Washington", "University of Washington") or NAICS codes (92xxxx = Public Administration).
- Starts 2016 — only 8 years of overlap with our 2000–2024 window.
- These are *advertised* salaries from job postings, not actual wages paid. Systematic upward bias likely.
- LinkedIn data only starts 2021, so pre-2021 coverage is Indeed-only (smaller sample, different employer mix).
- `ROLE_PRIMARY` being entirely null means the primary dataset alone lacks normalized job titles.
- Customization is available (can filter to WA only before download).

### Assessment
**Moderate value.** Can build a "private-sector advertised salary growth in Seattle metro" series for 2016–2024, filtered to comparable occupations via NAICS + Role Mapping. Would complement the BrightQuery occupation-level comparison but with less statistical rigor (posting salaries vs. survey wages). The effort to clean and filter may not be justified if BrightQuery covers the same ground more cleanly.

---

## 3. LinkUp — Job Records

**Slug:** `job-records`
**Partner:** LinkUp (a GlobalData company)

### Coverage
- **Geographic:** Global. Fields: `CITY`, `STATE`, `ZIP`, `COUNTRY`. Washington State coverage expected.
- **Temporal:** CREATED from 2007-08-03 to 2026-06-05 — **longest history** of any dataset surveyed.
- **Rows:** 346.5M | **Format:** Parquet | **Refresh:** Monthly

### Key Variables
- `TITLE` — raw job title from employer website
- `COMPANY_NAME`, `COMPANY_ID`
- `CREATED` / `DELETE_DATE` / `LAST_CHECKED` — can compute time-to-fill
- `URL` — original posting URL
- Supplementary: ONET Taxonomy (346M rows, O*NET code enrichment), Company Reference Tables

### Related: LinkUp Company Analytics
- Slug: `Company-Analytics`
- Aggregated job listing counts at the company-by-day level
- Temporal: 2007-08-03 to 2026-06-04
- Could show hiring volume trends for "State of Washington" as an employer

### Limitations
- **No salary or compensation data whatsoever.** This is purely job posting metadata.
- Scraped from employer career sites (not job boards), which skews toward larger, more sophisticated employers.
- Requires filtering by company name to identify state government postings.

### Assessment
**Low-to-moderate value.** No salary data means this can't directly support wage comparisons. Could build a labor-market-tightness narrative (e.g., "State of Washington posted X% more jobs in 2023 but GWI didn't keep pace") or time-to-fill analysis. The 2007+ history is appealing. Best as a sidebar visualization, not a core supplementary section.

---

## 4. RentHub — Rental Data [RECOMMENDED FOR HOUSING SECTION]

**Slug:** `rental-data-united-states`
**Partner:** RentHub

### Coverage
- **Geographic:** US-wide. Fields: `STATE`, `CITY`, `NEIGHBORHOOD`, `ZIP`, `LATITUDE`/`LONGITUDE`. 41,702 ZIP codes covered.
- **Temporal:** SCRAPED_TIMESTAMP from 2014-01-10 to 2026-05-26. **12+ years of data.**
- **Rows:** 535.7M | **Format:** Parquet, 56.8 GB | **Refresh:** Monthly

### Key Variables
- `RENT_PRICE` — advertised asking rent
- `BEDS`, `BATHS`, `SQFT`, `BUILDING_TYPE` (House/Condo/Apartment)
- `CITY`, `STATE`, `ZIP`, `NEIGHBORHOOD`
- Property features: `LAUNDRY`, `GARAGE`, `POOL`, `GYM`, etc.
- `DATE_POSTED` — original listing date

### Limitations
- Starts 2014, not 2000 — misses early portion of analysis window.
- **No MSA field** — requires a ZIP-to-MSA crosswalk to aggregate at metro level.
- Listing-level data (must aggregate to compute median rents).
- Scraped listings may include duplicates (same unit re-listed).
- Asking rent, not contracted rent — may be slightly higher than actual.

### Assessment
**Worth a supplementary section.** 12 years of listing-level rent data for Seattle-area ZIPs enables a compelling visualization: "A state employee earning the topped-out Range 40 salary could afford X% of 1BR listings in King County in 2014 vs. Y% in 2024." Tangible and relatable. Would pair with the CPI shelter sub-index from the core analysis.

---

## 5. Dwellsy — TotalIQ

**Slug:** `dwellsy-totaliq`
**Partner:** Dwellsy

### Coverage
- **Geographic:** US. Has `MSA_CODE`, `MSA_NAME`, `ADDRESS_STATE`, `ADDRESS_CITY` fields. Seattle MSA available.
- **Temporal:** CREATION_TS from 2020-09-03 to 2026-04-12. **Only 5.5 years.**
- **Rows:** 14.3M | **Format:** Parquet, 9.25 GB | **Refresh:** Monthly (weekly for MF+SFR variant)

### Key Variables
- `RENT_AMOUNT` — monthly asking rent
- `BEDROOMS`, `SQUARE_FEET`, `ADDRESS_TYPE`
- **Pre-computed MSA-level medians:** `0BED_APT_MEDIAN`, `1BED_APT_MEDIAN`, `2BED_APT_MEDIAN`, `2BED_HOUSE_MEDIAN`, `3BED_HOUSE_MEDIAN`, `4BED_HOUSE_MEDIAN`
- **Rent change percentages** for each type (e.g., `1BED_APT_RENT_CHANGE_PERCENTAGE`)
- `MSA_NAME` — direct MSA grouping (no crosswalk needed)

### Additional Products (Not Yet Explored)
- **Dwellsy TrendsIQ MSA** — pre-aggregated MSA-level trends (separate container/dataset)
- **Dwellsy TrendsIQ ZIP** — ZIP-level trends
- **Dwellsy TrendsIQ City** — city-level trends

### Limitations
- **Starts September 2020** — post-pandemic only. Too short for trend analysis against the 2000 or even 2014 baselines.
- Only 14M rows (much smaller sample than RentHub's 535M).
- Data sourced from PMS integrations, not full market coverage.

### Assessment
**Low value for this project due to temporal limitations.** The pre-computed MSA medians are convenient but the 2020+ start date means it only covers the post-COVID rental spike — which is interesting but not the long-term story we're telling. If we use RentHub for the housing section, Dwellsy adds nothing that couldn't be derived. The TrendsIQ products might offer ready-made trend summaries worth checking in a future session.

---

## 6. SafeGraph — Spend Patterns

**Slug:** `spend`
**DOI:** https://doi.org/10.82551/NSF5-R186
**Citation:** SafeGraph. (2022). Spend Patterns [Dataset]. Dewey Data.
**Partner:** SafeGraph

### Coverage
- **Geographic:** US-only. Fields: `REGION` (state abbreviation — "WA" confirmed), `CITY`, `POSTAL_CODE`. POI-level with lat/long.
- **Temporal:** SPEND_DATE_RANGE_START from 2019-01-01 to 2026-04-01. **7+ years.**
- **Rows:** 93.7M | **Format:** Parquet | **Refresh:** Monthly

### Key Variables
- `NAICS_CODE` and `TOP_CATEGORY`/`SUB_CATEGORY` — 1,441 distinct NAICS codes, 348 top categories, 967 sub-categories
- `RAW_TOTAL_SPEND`, `RAW_NUM_CUSTOMERS`, `RAW_NUM_TRANSACTIONS`
- `MEDIAN_SPEND_PER_CUSTOMER`, `MEDIAN_SPEND_PER_TRANSACTION`
- `SPEND_PCT_CHANGE_VS_PREV_YEAR` — year-over-year spend change
- `BUCKETED_CUSTOMER_INCOMES` — income distribution of customers
- Cross-shopping and related brand data

### Relevant NAICS Categories for This Project
- **Grocery:** NAICS 4451xx (Grocery Stores), 4452xx (Specialty Food Stores)
- **Healthcare:** NAICS 6211xx (Offices of Physicians), 6216xx (Home Health Care), 621xxx broadly
- **Gas:** NAICS 4471xx (Gasoline Stations)
- **Childcare:** NAICS 6244xx (Child Day Care Services)

### Limitations
- Starts 2019 — only 7 years, all post-CPI-spike.
- Credit/debit card panel data — cash transactions invisible, and panel composition may shift over time.
- POI-level data requires aggregation by category and geography.
- Spend amounts are panel-level, not population-level — trends are directionally useful but absolute dollar amounts are not directly comparable to CPI.
- Terms of use restrict financial-investment use cases (not relevant here, but noted).

### Assessment
**Moderate value as a tangibility layer.** Can show "median grocery spend per transaction at King County stores rose X% from 2019 to 2024" alongside CPI food sub-index data. Adds experiential texture to the CPI argument. However, the short time window and panel-data limitations mean it can only supplement, not replace, the BLS CPI sub-index analysis. Best used for 1–2 specific vignettes rather than a full section.

---

## Prioritized Recommendation

| Priority | Dataset | Use Case | Effort | Time Range |
|----------|---------|----------|--------|------------|
| **1** | BrightQuery Occupational Wage | Public vs. private sector wage comparison by occupation in Seattle MSA | Medium | 2010–present |
| **2** | RentHub Rental Data | Housing affordability over time for state employee salary ranges | Medium-High | 2014–present |
| **3** | SafeGraph Spend Patterns | Tangible cost-of-living vignettes (grocery, healthcare spend trends) | Low-Medium | 2019–present |
| **4** | WageScape Job Postings | Private-sector advertised salary growth benchmark | High | 2016–present |
| **5** | LinkUp Job Records | Labor market tightness / hiring competition narrative | Medium | 2007–present |
| **6** | Dwellsy TotalIQ | Recent rent trends (overlaps with RentHub) | Low | 2020–present |

### Next Steps
1. **BrightQuery:** Sample data to confirm WA state government occupations are present and map SOC codes to classified job titles (IT, admin, fiscal analyst, etc.)
2. **RentHub:** Sample Seattle-area listings to assess data quality and plan ZIP-to-MSA aggregation
3. **SafeGraph:** Sample King County grocery/healthcare POIs to check data density
4. Decide which supplementary sections to add to the notebook before downloading any data

### Data Governance Reminder
Per `.claude/rules/data-governance.md`: Dewey datasets are UW-licensed. Raw data files must **never** be committed to this repository. Only derived statistics (medians, percentiles, growth rates) with source attribution belong in the notebook or `data/processed/` directory.
