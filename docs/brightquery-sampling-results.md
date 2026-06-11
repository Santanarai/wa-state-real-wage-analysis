# BrightQuery Occupational Wage Data — Sampling Results

**Date:** 2026-06-11
**Dataset:** BrightQuery Occupational Wage Data (Dewey slug: `occupational-wage-data`)
**Purpose:** Feasibility check for a supplementary notebook section comparing state government vs. private-sector wages at the occupation level in the Seattle MSA.
**Data governance:** Tier 2 (UW-licensed). This document contains only derived observations and summary statistics — no raw data rows.

---

## Sampling Methodology

### Tool constraints
The Dewey `sample_dataset` MCP tool returns a fixed, deterministic sample of rows — not a random draw. Repeated calls with identical parameters return the same rows (confirmed by identical `externalId: sfdsp_kf9go9amiueaxsdu` across 5 calls). The effective maximum is **100 rows** regardless of the requested `n` parameter (tested with `n=2000`; `sampleRowsCount=100` in dataset metadata).

### Calls made
| Call | Tool | Parameters | Rows returned |
|------|------|-----------|--------------|
| 1 | `get_dataset_schema` | `slug=occupational-wage-data` | N/A (schema) |
| 2 | `sample_dataset` | `n=2000, format=json, 11 columns` | 100 |
| 3–6 | `sample_dataset` | Same parameters | 100 (identical to call 2) |

**Total unique rows:** 100 out of 25.5M (0.0004% of dataset). No method exists to expand sample coverage via the Dewey MCP — the sample is fixed.

### Columns requested
`BQ_AREA_TYPE`, `BQ_AREA_CD`, `BQ_AREA_DESCRIPTION`, `BQ_ADDRESS_STATE`, `BQ_OCC_OWNERSHIP_CD`, `BQ_OCC_CD`, `BQ_OCC_DESCRIPTION`, `BQ_YEAR`, `BQ_OCC_ANNUAL_MEDIAN_WAGE`, `BQ_NAICS_CODE`, `BQ_OCC_INDUSTRY_LEVEL`

---

## Schema Confirmation

The schema matches the reconnaissance doc with corrections noted below.

**33 columns total.** All column names confirmed as documented. Key types:
- `BQ_AREA_TYPE`, `BQ_AREA_CD`, `BQ_YEAR`: integer (returned as string in sample)
- `BQ_OCC_OWNERSHIP_CD`, `BQ_OCC_CD`, all wage fields: string (TEXT)
- Wage fields are comma-formatted strings (e.g., `"51,270"`, `"97,910"`)

### Schema corrections vs. reconnaissance doc
1. **Year range is 2011–2023, not "2010–present."** Schema metadata shows `minRaw: "2011"`, `maxRaw: "2023"`, with 13 distinct values. The sample itself shows years 2012–2020 only (sampling artifact).
2. **`BQ_ADDRESS_STATE` is heavily null.** Approximately 69% of rows in the full dataset have null values for this field. In the sample, MSA-level and earlier-year rows show `"None"` while some 2020+ rows are populated. For geographic filtering, use `BQ_AREA_CD` (CBSA code) and `BQ_AREA_DESCRIPTION` instead.
3. **`BQ_OCC_OWNERSHIP_CD` is stored as TEXT with many combined codes.** The schema reports this is NOT marked as categorical. Beyond the simple codes (1, 2, 3, 5), combined codes like `1235` (all sectors), `235`, `35`, `57`, `58`, `59` represent aggregate ownership groupings. The sample is dominated by `1235` (cross-sector combined).
4. **Area type 5 exists (Metropolitan Division)** — undocumented in the field description but confirmed in the sample (e.g., `Dallas-Plano-Irving, TX Metropolitan Division`).

---

## Distribution Findings (100-row sample)

### Area type distribution
| Area type | Description | Count | Pct |
|-----------|-------------|-------|-----|
| 1 | U.S. (national) | 28 | 28% |
| 2 | State | 10 | 10% |
| 4 | MSA | 46 | 46% |
| 5 | Metropolitan Division | 3 | 3% |
| 6 | Nonmetropolitan | 13 | 13% |

### Ownership code distribution
| Code | Meaning | Count | Pct |
|------|---------|-------|-----|
| 1235 | All sectors combined | 73 | 73% |
| 5 | Private | 22 | 22% |
| 2 | **State Government** | 1 | 1% |
| 3 | Local Government | 1 | 1% |
| 1 | Federal Government | 1 | 1% |
| 35, 57, 58, 59, 123, 235 | Other combinations | 0 | 0% |

### SOC codes observed
Only 2 occupation codes appeared in the 100-row sample:
- **25-0000** — Education, Training, and Library Occupations (major group)
- **17-3011** — Architectural and Civil Drafters (detailed)

**None of the 6 target SOC families appeared** (43-xxxx admin, 15-xxxx IT, 13-xxxx accounting, 19-xxxx science, 31-xxxx healthcare, 43-1011 supervisors). This is expected given 100 rows sampling from 800+ occupations — absence in the sample is not evidence of absence in the dataset.

### Year range in sample
Years 2012–2020 observed. Years 2011, 2021, 2022, 2023 did not appear in the sample (sampling artifact — full dataset confirmed to have 2011–2023 from schema metadata).

---

## State Government Data

### Confirmed present
One row with `BQ_OCC_OWNERSHIP_CD = "2"` (State Government) was found:
- **Area:** U.S. national (area_type 1)
- **Occupation:** SOC 25-0000 (Education, Training, and Library Occupations)
- **Year:** 2013
- **Industry:** NAICS 622300 (Specialty Hospitals)
- **Median wage:** `"1,53,830"` — **data quality issue** (appears to be Indian-format number or corruption; expected value for this cell is likely $53,830)

This confirms:
- Ownership code "2" exists in the dataset and is populated
- State government data appears at the national-industry level (area_type=1, NAICS-specific)
- Whether ownership code "2" appears at the **geographic level** (area_type 2=State or 4=MSA) could not be confirmed from this sample

### Structural expectation
The BLS OEWS publishes state government estimates at both the state level (area_type=2) and MSA level (area_type=4) when sample sizes permit. Since BrightQuery normalizes OEWS data, geographic-level state government data should exist. The 100-row sample is simply too small to surface Seattle MSA + State Government combinations (~0.01% of the dataset).

---

## Washington State / Seattle MSA Data

### WA-related rows found
| Area | Area code | Type | Occupation | Year | Ownership | Wage |
|------|-----------|------|-----------|------|-----------|------|
| Bellingham, WA | 13380 | MSA (4) | Arch/Civil Drafters | 2017 | 1235 (combined) | $59,690 |
| NW Washington nonmetro | 5300001 | Nonmetro (6) | Education occupations | 2012 | 5 (Private) | $42,800 |

**Seattle-Tacoma-Bellevue MSA (CBSA 42660):** Not present in the 100-row sample. Expected — Seattle is one of ~400 MSAs, each with multiple occupation×year×ownership combinations. A 100-row sample has ~11% probability of including any Seattle row.

### Seattle MSA CBSA code
Seattle-Tacoma-Bellevue, WA MSA uses CBSA code **42660**. This was not confirmed from the sample but is the standard BLS/Census designation. The `BQ_AREA_CD` field should contain `42660` for Seattle MSA rows in the full dataset.

---

## Proof-of-Concept Wage Ratio

**Not achievable from this sample.** A matched pair requires the same SOC code + same area + same year with different ownership codes (2 vs. 5). The sample contains only 1 state-government row and 1 local-government row, neither of which has a matching private-sector counterpart at the same area/occupation/year.

However, the data structure confirms the comparison is mechanically feasible: the `BQ_OCC_OWNERSHIP_CD` field separates ownership types, wage fields are populated for all ownership codes observed, and the NAICS `000000` / industry level `cross-industry` pattern identifies geographic-level (non-industry-specific) estimates suitable for public/private comparisons.

---

## Feasibility Determination

### GO — Conditional

The occupation-level public/private wage comparison section is structurally feasible. The BrightQuery dataset contains the required fields: ownership-stratified wage percentiles by occupation (SOC code), geography (MSA), and year. However, the Dewey sampling tool cannot provide the granularity needed for analysis — a full data download or direct BLS access is required.

### Criteria assessment
| Criterion | Status | Evidence |
|-----------|--------|----------|
| Ownership codes confirmed | **PASS** | Codes 1, 2, 3, 5, 1235 all observed |
| Wage fields parseable | **PASS** | Comma-formatted numerics; one quality issue noted |
| SOC code structure confirmed | **PARTIAL** | XX-XXXX format confirmed; target families not in sample (expected) |
| Temporal range confirmed | **PASS** | 2011–2023 from schema metadata |
| Data source pathway clear | **PASS** | BLS OEWS public data available as Tier 1 alternative |

### Data quality flag
The `"1,53,830"` wage value on the state-government row uses non-standard formatting (double comma). This may indicate data corruption in the BrightQuery normalization or an edge case in number formatting. The BLS source data should not have this issue — verify during full analysis.

---

## Recommended Data Source

### Primary recommendation: BLS OEWS public data (Tier 1)

The BrightQuery dataset is derived from BLS Occupational Employment and Wage Statistics (OEWS). The public OEWS data is:
- **Freely downloadable** from bls.gov/oes/tables.htm as annual research files
- **Tier 1** under project data governance (public government data — can be committed freely)
- **Same content:** occupation × area × ownership wage percentiles
- **Filterable:** Download the complete file and filter to CBSA 42660, ownership types 2 and 5
- **Coverage:** Annual releases, currently 2012–2023 (each release covers a 3-year rolling survey period)

**Advantages over BrightQuery/Dewey:**
1. No UW license dependency — shareable, reproducible, committable
2. No Dewey sampling constraints — full access to all ~800K rows per year
3. Direct BLS provenance — no normalization intermediary to validate
4. No data quality concerns from BrightQuery's formatting pipeline

**Disadvantage:** Requires downloading and processing annual flat files (~50–100 MB each) rather than a pre-normalized time series. This is minor — a pandas script can merge and filter the annual files.

### When to use BrightQuery instead
If the team needs a quick, pre-joined time series across all years without building the merge pipeline, and has UW institutional Dewey access for the full download. BrightQuery's normalization does save preprocessing time.

---

## Next Steps (for future sessions)

1. **Download BLS OEWS research files** for 2012–2023 from bls.gov/oes/tables.htm
2. **Filter to:** MSA 42660 (Seattle-Tacoma-Bellevue) + ownership codes 2 (State Gov) and 5 (Private)
3. **Identify SOC code crosswalk** from WA classified job titles to SOC codes (starting with the 6 target families)
4. **Compute wage ratios** by occupation for the earliest and most recent available years
5. **Build supplementary notebook section** showing occupation-level divergence over time

---

## Citation

BrightQuery. (2022). Occupational Wage Data (Version 1.0) [Dataset]. Dewey Data.
Sampling performed 2026-06-11 via Dewey MCP `sample_dataset` tool.
