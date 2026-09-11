# digiryte-challenge1-nhs-ae-analysis
Analysis of NHS England A&amp;E attendance data (Oct 2025-Mar 2026) — 3 findings on winter pressure, wait-time normalization, and trust size vs performance. Built for Digiryte's Round 3 technical challenge.
# NHS A&E Attendance Analysis — Digiryte Round 3, Challenge 1

## Overview
A focused analysis of NHS England's official A&E (Accident & Emergency) attendance
data covering October 2025 – March 2026, produced for Digiryte's Round 3 technical
challenge.

**Why this dataset:** Chosen for direct relevance to Digiryte's client work with the
NHS — this uses the same kind of real, operational public-sector data the company's
healthcare projects deal with, rather than a generic Kaggle dataset.

## What's in this repo
```
├── NHS_AE_Challenge1_Analysis.ipynb   # main notebook: cleaning + 3 findings
├── data/                              # 6 monthly raw CSVs (Oct 2025 - Mar 2026)
├── finding1_breach_rate_trend.png     # chart: national breach rate trend
├── finding2_12hr_wait_rate.png        # chart: worst trusts by 12+ hr wait rate
├── finding3_size_vs_breach.png        # chart: trust size vs breach rate
└── README.md
```

## Data source
NHS England, A&E Attendances and Emergency Admissions:
https://www.england.nhs.uk/statistics/statistical-work-areas/ae-waiting-times-and-activity/

Six monthly CSVs (Oct 2025 – Mar 2026) are included as downloaded, unmodified, in `data/`.

## How to run
1. Open `NHS_AE_Challenge1_Analysis.ipynb` in Google Colab (or Jupyter locally)
2. If running locally, install dependencies: `pip install pandas matplotlib`
3. Upload the 6 CSVs from `data/` into the same working directory as the notebook
4. Run all cells top to bottom — the notebook combines, cleans, and analyzes the data,
   producing all 3 findings and their charts

## Data quality issues found and handled
1. **Summary rows (`Period == "TOTAL"`):** Each monthly file included a national
   summary row alongside individual trust rows. Removed to avoid double-counting.
2. **Hidden rollup row (`Org Code == "TOTAL"`):** February's file additionally
   contained a second aggregation row with a normal `Period` value but
   `Org Code == "TOTAL"`, which the first cleaning pass missed. This was caught
   because February's national attendance total looked nearly double every other
   month — traced back to this row and removed consistently across all months.
3. **Inconsistent org reporting:** Two org codes (NNJ10, NNJ11) appeared in only
   1 of 6 months — likely a trust merge/rename mid-period. Kept as-is since it's a
   legitimate reporting gap, not a data error.
4. **Zero-attendance rows:** 458 rows had 0 Type 1 (major A&E) attendances — these
   are legitimate, mostly minor walk-in centres/GP practices that don't run a full
   A&E department, not missing data.

After cleaning: 1,184 rows across 6 months, no duplicates, no missing values.

## Findings

**1. Winter Pressure Is Real, and Sharp**
National A&E breach rates rose from 39.9% (Nov 2025) to a peak of 42.96% (Jan 2026),
then fell to 36.11% by March — corroborated independently by a 41% spike in 12+ hour
waits over the same period. However, this national average hides sharp trust-level
variation — some mid-sized trusts (e.g. Chesterfield Royal Hospital, 72.9% breach rate)
performed far worse than the national figure, suggesting winter pressure isn't evenly
distributed.

**2. Raw Counts Mislead — Normalize by Size**
By raw count, the worst 12+ hour wait totals belong to the biggest trusts (Royal Free
London, Birmingham) — unsurprising given their volume. Normalized as a % of emergency
admissions, a different trust dominates: Croydon Health Services, where 62.8% of
emergency admissions waited 12+ hours.

**3. Trust Size Doesn't Predict Performance**
There is effectively no correlation (r = -0.083) between attendance volume and breach
rate. Both the best (Sheffield Children's, 8.5% breach rate) and worst (North Tees,
55.2%) performers are similarly-sized smaller trusts, while the largest trust
(Birmingham, 204K attendances) performs close to average.

## Assumptions
- Used Type 1 (major A&E) attendance and breach figures as the primary metric, since
  this represents full emergency departments rather than minor injury units/walk-in
  centres.
- Excluded orgs with fewer than 5,000 total attendances over the period from the
  size-vs-performance analysis (Finding 3) to avoid noise from very small or
  specialist units skewing the correlation.

## What I'd investigate next with more time
- Whether trusts with consistently poor performance (e.g. Chesterfield, Croydon)
  share common factors — staffing ratios, bed capacity, local demographic pressure
- Whether the January performance dip is driven by higher demand or by reduced
  capacity (e.g. staff sickness/holiday coverage over the winter period)
- Extending the time window beyond 6 months to check whether these patterns hold
  year-over-year

