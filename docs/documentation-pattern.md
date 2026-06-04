# Notebook Documentation Pattern

Every analytical section in the notebook follows this five-part structure. This pattern exists because the analysis is AI-assisted, and the notebook must demonstrate that a human with domain knowledge validated every step.

## The Five Parts

### 1. Objective
What question this section answers, in plain language. This is where Gabe's domain knowledge lives — framing the right question is the human contribution.

**Example:** "Calculate cumulative real-wage change for WA General Service employees against Seattle-area CPI-U, 2020–2026."

### 2. Instruction
What analytical decision was made and what Claude Code was asked to do. Not the raw prompt — the reasoning behind the approach chosen.

**Example:** "Pull BLS series CUURS49DSA0 annual averages. Pull OFM GWI history for general government. Compute chained real-wage index with 2020 as base year = 100. Use CPI-U rather than CPI-W because [reason]."

When a methodological choice exists (CPI-U vs CPI-W, annual average vs December-to-December, base year 2000 vs 2020), document the choice and the reasoning. If both options are defensible, compute both and present side by side.

### 3. Output
The code and its results, as generated. Requirements:
- Code must be readable and commented
- Source URLs and BLS series IDs appear as comments in data-retrieval cells
- Chart source citations appear directly on the figure (e.g., "Source: BLS, Series CUURS49DSA0")
- Every chart has clear axis labels, title, and legend

### 4. Validation
How the output was verified. **This is the most important cell.** It is the answer to "how do we know AI did this right?"

Validation methods (use at least one per section):
- **Spot-check**: Pick a specific year/value and confirm it matches the BLS website or OFM schedule
- **Sanity check**: Does the magnitude make sense? A 50% real-wage loss would be implausible; a 5-15% loss is plausible for this time period
- **Cross-reference**: Compare against a known reference (e.g., WPEA's 21% figure, OFM's -18.2% market lag)
- **Reversal test**: If the calculation were run backward (deflating CPI by wages instead of wages by CPI), does the inverse relationship hold?
- **Pay-stub check**: Does the nominal wage for a specific range/step match what the author actually received?

If a value was wrong and corrected, document what was wrong, how it was caught, and what the corrected value is. Do not silently fix errors.

### 5. Interpretation
What the output means in plain language for the bargaining team. This is the "so what?" cell.

**Example:** "Classified employees on the General Service scale received cumulative nominal raises of X% from 2020–2026. Seattle-area CPI rose Y% over the same period. The real-wage gap is Z%, equivalent to approximately $W per year in lost purchasing power for a Range 40 Step L employee."

No jargon. No hedging. Clear, opinionated statement of what the data shows — but stay within the data. Editorial claims about what the union should demand belong to the union, not this analysis.
