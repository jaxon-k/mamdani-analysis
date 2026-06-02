# NYC Mayoral Primary Analysis: Mamdani Vote Share by Precinct

A precinct-level analysis of Zohran Mamdani's performance in the 2026 New York City Democratic mayoral primary, examining the demographic and partisan drivers of his vote share across New York City's election districts.

# Github repo link: https://github.com/jaxon-k/mamdani-analysis
---

## Project Overview

This project was completed as a technical assessment. The central question: **what explains Mamdani's vote share at the precinct level, and where did he over- or underperform relative to expectations?**

The analysis proceeds in four stages:

1. **Basic performance picture** — vote share breakdown by county and assembly/election district for all three major candidates (Mamdani, Cuomo, Sliwa)
2. **Partisan baseline comparison** — modeling Mamdani's expected vote share from 2024 Democratic performance, then computing residuals to identify precincts where he outperformed or underperformed that baseline
3. **Demographic deep-dive** — exploring relationships between Mamdani's vote share and age, race, and ethnicity at the precinct level using scatter plots and visual inspection
4. **Predictive model** — an OLS regression modeling Mamdani's vote share as a function of partisan baseline, age composition, and racial demographics

---

## Data Source

The dataset is a **GeoJSON FeatureCollection** of 3,991 election district polygons covering New York City, compiled by VoteHub. Each feature represents a single election district and includes:

| Category | Fields |
|---|---|
| **2026 primary results** | Raw vote totals for Mamdani, Cuomo, and Sliwa; total votes cast |
| **2024 general election estimates** | Estimated vote totals by party for President, Senate, House, and Governor races |
| **Voter file demographics (2020 cycle)** | Registered voter counts by race/ethnicity (`v_20_c_*`) and voter turnout by race (`v_20_v_*`) |
| **Age distribution** | Estimated population by 5-year age band (2022 ACS, `X_22_2022_Age_Expanded_*`) |
| **Socioeconomic** | Median income (`med_ncm`), college attainment (`bch_pl_`) |
| **Geography** | Assembly District (`ad`), Election District (`ed`), County, precinct ID, and MultiPolygon geometry |

---

## Methods

### Partisan Baseline & Residual Analysis
Mamdani's expected vote share in each election district was modeled as a linear function of the estimated 2024 Democratic presidential vote share. Residuals from this model identify districts where he meaningfully over- or underperformed the Democratic baseline — useful for isolating the effect of candidate-specific factors beyond partisanship.

### Demographic Analysis
Precinct-level demographic shares (race/ethnicity, proportion of voters under 35) were correlated visually and statistically with Mamdani's vote share. Outlier districts from the residual analysis are highlighted across all demographic plots.

### OLS Regression Model
The final predictive model estimates Mamdani's vote share as:

$$\text{VoteShare}_{Mamdani} = \beta_0 + \beta_1(\text{DemShare}) + \beta_2(\text{Under35}) + \beta_3(\text{Asian}) + \beta_4(\text{Black}) + \beta_5(\text{Hispanic}) + \beta_6(\text{White}) + u_i$$

Model performance is evaluated on a held-out test set (80/20 split) using RMSE and R².

---

## Key Findings

- **Partisan alignment was the strongest predictor** of Mamdani's vote share (coefficient ~0.75), suggesting he held the Democratic base effectively despite running against a fellow Democrat
- **Younger districts favored Mamdani** — a clear positive relationship between the proportion of voters under 35 and his vote share
- **Whiter precincts trended away from Mamdani** — a negative relationship was visible across the scatter plots, consistent with Cuomo's stronger performance in moderate and white Democratic neighborhoods
- **Black and Hispanic precincts showed no clear linear relationship** — suggesting more heterogeneous support patterns in these communities that a simple linear model does not fully capture
- **Assembly District 26 (Queens)** was among the strongest underperformers relative to the Democratic baseline, consistent with Cuomo's dominance in moderate outer-borough precincts

---

## Repository Structure

```
mamdani-analysis/
├── README.md
├── analysis.ipynb        # Full analysis notebook
├── writeup.pdf           # Written summary of findings
└── data/
    └── README.md         # Data dictionary and source documentation
```

---

## Dependencies

```
pandas
numpy
geopandas
matplotlib
seaborn
scikit-learn
```
