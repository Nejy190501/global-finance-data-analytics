---
layout: default
title: Analysis Plan
nav_order: 2
description: "Per-sub-question breakdown of variables, filters, computations, and expected outputs."
---

# Concrete Analysis Plan — What Data to Use and How

This document maps each sub-question to the exact columns from `df_panel`, the filters to apply, the metrics to compute, and the output each step should produce.

**Panel reminder:** `df_panel` — 3,831 rows, 216 countries × 18 years (2004–2021)

| Column | Coverage |
|---|---|
| `gdp_growth` | 98% |
| `inflation` | 97% |
| `fdi_inflows` | 90% |
| `bank_branches` | 82% |
| `atms` | 78% |
| `domestic_credit` | 78% |
| `broad_money` | 73% |
| `interest_spread` | 57% |
| `nonperforming_loans` | 48% |
| `market_cap` | 35% |

Metadata columns available after joining `Metadata_Country.csv` on `Country Code`: `Region`, `IncomeGroup`.

**Primary variables for most sub-questions:** `gdp_growth`, `domestic_credit`, `broad_money`, `bank_branches`, `atms`, `fdi_inflows`, `inflation`.
**Supporting variables (use with caution):** `interest_spread`, `nonperforming_loans`.
**Avoid as primary:** `market_cap` (too many gaps, especially in lower-income countries).

---

## Preparation steps (run once before any sub-question)

1. Re-run the cleaning pipeline from `data_overview_plus_merging.ipynb` to get `df_panel`.
2. Join country metadata:
   ```python
   df = df_panel.merge(country_meta[["Country Code", "Region", "IncomeGroup"]], ...)
   ```
3. Define period labels as a new column:
   ```
   pre_2008:   2004–2007
   crisis_08:  2008–2009
   recovery:   2010–2019
   covid:      2020–2021
   ```
4. Define income-group order for plotting:
   ```
   ["Low income", "Lower middle income", "Upper middle income", "High income"]
   ```

---

## SQ1 — Mapping the financial landscape

**Core question:** How do financial-development profiles differ across income groups and regions?

### Variables to use
`domestic_credit`, `broad_money`, `bank_branches`, `atms`, `fdi_inflows` (the five best-covered financial indicators, excluding the outcome `gdp_growth` and the control `inflation`)

### Data subset
Full panel, all years (2004–2021). No year filter — we want the structural picture over the whole window.

### Step-by-step

**Step 1 — Median profile per income group**
```
group by IncomeGroup
compute: median of each of the 5 variables
output: a 4×5 table (income group × indicator)
visualization: heatmap or grouped bar chart
```

**Step 2 — Median profile per region**
```
group by Region
compute: median of each of the 5 variables
output: a 7×5 table
visualization: heatmap
```

**Step 3 — Trends over time by income group**
```
group by [IncomeGroup, Year]
compute: median of domestic_credit and bank_branches (the two most policy-relevant)
visualization: two line charts, one per variable, four lines per chart (one per income group)
this shows whether profiles are converging or diverging over 2004–2021
```

**Step 4 — Clustering (exploratory)**
```
compute per-country average of the 5 variables over 2004–2021 (one row per country)
drop countries with more than 2 of 5 variables missing
normalize each variable (zero mean, unit std)
run k-means with k=4 (matching the 4 income groups) and also k=3, k=5
compare cluster labels vs IncomeGroup labels (confusion matrix or cross-tab)
question to answer: does the data naturally group countries the same way the WB does?
```

### Output
- Table: median financial profile per income group
- Chart: time-series of domestic credit by income group (2004–2021)
- Cross-tab: cluster label vs income group

---

## SQ2 — Ranking the channels in normal times

**Core question:** Which financial indicator is most associated with GDP growth, and does the ranking shift across income groups?

### Variables to use
Outcome: `gdp_growth`
Channels: `domestic_credit`, `broad_money`, `bank_branches`, `atms`, `fdi_inflows`
Control: `inflation`

### Data subset
**Exclude crisis years:** drop rows where `Year` is in `[2008, 2009, 2020, 2021]`.
This leaves years 2004–2007 and 2010–2019 as the "normal times" baseline (14 years).

```python
normal = df[~df["Year"].isin([2008, 2009, 2020, 2021])]
```

### Step-by-step

**Step 1 — Global correlation table**
```
compute Pearson and Spearman correlation of each channel vs gdp_growth
on the full normal-times subset
output: ranked table — which channel correlates most with growth globally?
visualization: bar chart of correlation coefficients, one bar per channel
```

**Step 2 — Correlation by income group**
```
for each income group:
    subset to that group
    compute Pearson correlation of each channel vs gdp_growth
output: 4×5 table (income group × channel), correlation coefficients
visualization: heatmap — rows = income groups, cols = channels, values = correlation
this directly answers "does the ranking differ?"
```

**Step 3 — Multivariate OLS regression (pooled)**
```
y = gdp_growth
X = [domestic_credit, broad_money, bank_branches, atms, fdi_inflows, inflation]
run on normal-times subset
use statsmodels OLS, report coefficients and p-values
purpose: isolate each channel's marginal contribution after controlling for the others
```

**Step 4 — OLS per income group (optional, if time allows)**
```
repeat step 3 inside each income group
compare which coefficients are significant per group
```

### Output
- Ranked global correlation table
- Heatmap: correlations by income group × channel
- OLS summary table (global)

---

## SQ3 — Behaviour during the 2008 global financial crisis

**Core question:** How did the finance-growth relationship shift around 2008, and which financial characteristics separated resilient countries from those hit hardest?

### Variables to use
Outcome: `gdp_growth`
Channels: `domestic_credit`, `broad_money`, `bank_branches`, `atms`, `fdi_inflows`
Grouping: `IncomeGroup`, `Region`

### Data subset
Focus years: 2004–2014 (enough pre- and post-shock coverage). Divide into:
- Pre-crisis: 2004–2007
- Crisis: 2008–2009
- Recovery: 2010–2014

```python
df_08 = df[df["Year"].between(2004, 2014)].copy()
df_08["period_08"] = pd.cut(df_08["Year"], bins=[2003,2007,2009,2014],
                             labels=["pre","crisis","recovery"])
```

### Step-by-step

**Step 1 — Average GDP growth by period and income group**
```
group by [IncomeGroup, period_08]
compute: mean gdp_growth
visualization: grouped bar chart (4 income groups × 3 periods)
shows the shock magnitude and recovery speed per income tier
```

**Step 2 — Country-level deviation from pre-crisis trend**
```
for each country:
    compute average gdp_growth in pre-crisis window (2004–2007)
    compute gdp_growth in 2009 (worst crisis year)
    deviation = gdp_growth_2009 - pre_crisis_avg
output: one deviation score per country (negative = fell below trend)
```

**Step 3 — Resilient vs. hard-hit country profiles**
```
rank countries by deviation score
define: top quartile = most resilient (smallest drop), bottom quartile = hardest hit
for each group:
    compute median of domestic_credit, broad_money, bank_branches, atms, fdi_inflows
    in the pre-crisis years (2004–2007)
output: side-by-side profile table — what did resilient countries look like financially before the crisis?
visualization: radar chart or bar chart comparing the two profiles
```

**Step 4 — Time series chart**
```
group by [IncomeGroup, Year], compute mean gdp_growth
line chart 2004–2014, four lines (one per income group)
add vertical bands for crisis years 2008–2009
```

### Output
- Grouped bar chart: avg growth by income group × period
- Profile table: resilient vs hard-hit countries' pre-crisis financial characteristics
- Line chart: growth over 2004–2014 by income group

---

## SQ4 — COVID-19 shock and comparison to 2008

**Core question:** How did COVID-2020 affect countries differently, and how does the recovery compare to 2009–2010?

### Variables to use
Same as SQ3: `gdp_growth`, `domestic_credit`, `broad_money`, `bank_branches`, `atms`, `fdi_inflows`

### Data subset
Focus years: 2017–2021. Divide into:
- Pre-COVID: 2017–2019
- Shock: 2020
- Recovery: 2021

```python
df_covid = df[df["Year"].between(2017, 2021)].copy()
df_covid["period_covid"] = pd.cut(df_covid["Year"], bins=[2016,2019,2020,2021],
                                   labels=["pre","shock","recovery"])
```

### Step-by-step

**Step 1 — Same groupby as SQ3, applied to COVID window**
```
group by [IncomeGroup, period_covid]
compute: mean gdp_growth
visualization: grouped bar chart (same format as SQ3 chart — enables direct visual comparison)
```

**Step 2 — Country-level deviation (same method as SQ3)**
```
pre-COVID avg: 2017–2019
deviation = gdp_growth_2020 - pre_covid_avg
output: deviation score per country
```

**Step 3 — Resilient vs hard-hit profiles (same method as SQ3)**
```
top/bottom quartile by 2020 deviation
compare pre-COVID financial profiles
note: are the same countries in the resilient group in both crises?
```

**Step 4 — Direct comparison table: 2008 crisis vs COVID crisis**
```
for each income group, compare across both crises:
    peak drop in gdp_growth (worst year)
    recovery speed (years to return to pre-crisis avg)
    which financial variable most distinguished resilient countries
output: 4×3 comparison table (income group × [2008 peak drop, COVID peak drop, key differentiator])
```

**Step 5 — Overlay line chart**
```
plot two line charts side by side:
    left: avg gdp_growth by income group, 2004–2012 (centred on 2009)
    right: avg gdp_growth by income group, 2016–2021 (centred on 2020)
same y-axis scale to make the size of each shock visually comparable
```

### Output
- Grouped bar chart: COVID growth by income group × period
- Comparison table: 2008 vs COVID on key dimensions
- Side-by-side shock chart

---

## SQ5 — The shift from branches to ATMs

**Core question:** How is the structure of financial access changing, and is the shift tracking growth outcomes in lower-income economies?

### Variables to use
Primary: `bank_branches`, `atms` (both per 100,000 adults)
Outcome: `gdp_growth`
Filter: focus on `Low income` and `Lower middle income` for the growth-linkage step

### Step-by-step

**Step 1 — Global trend of branches vs ATMs over time**
```
group by [Year]
compute: median bank_branches, median atms
line chart: two lines (branches, ATMs) over 2004–2021 on the same axis
this shows the global structural shift
```

**Step 2 — Same trend by income group**
```
group by [IncomeGroup, Year]
compute: median bank_branches, median atms
4 small charts (one per income group), each with two lines
shows whether the substitution is faster in some income tiers than others
```

**Step 3 — Compute substitution speed per country**
```
for each country with data in both 2004 and 2021:
    atm_branch_ratio_start = atms_2004 / bank_branches_2004
    atm_branch_ratio_end   = atms_2021 / bank_branches_2021
    substitution_speed = atm_branch_ratio_end - atm_branch_ratio_start
output: one substitution speed score per country
handle: if bank_branches is 0 or very small, cap the ratio to avoid division artefacts
```

**Step 4 — Top substituters vs slow substituters**
```
rank countries by substitution speed
show top 10 and bottom 10 (bar chart)
label by income group — are fast substituters concentrated in a particular tier?
```

**Step 5 — Substitution speed vs GDP growth (lower-income focus)**
```
subset: Low income + Lower middle income only
x-axis: substitution_speed (2004–2021 change in atm/branch ratio)
y-axis: average gdp_growth over 2004–2021
scatter plot with country labels for outliers
add Pearson correlation and a linear fit line
question: in lower-income economies, does faster ATM expansion relative to branches track better growth?
```

### Output
- Line chart: global branches vs ATMs over time
- Small multiples: branches vs ATMs by income group
- Scatter: substitution speed vs average GDP growth (lower-income subset)

---

## Variable usage summary

| Variable | SQ1 | SQ2 | SQ3 | SQ4 | SQ5 |
|---|---|---|---|---|---|
| `gdp_growth` | | outcome | outcome | outcome | outcome |
| `domestic_credit` | profile | channel | resilience | resilience | |
| `broad_money` | profile | channel | resilience | resilience | |
| `bank_branches` | profile | channel | resilience | resilience | primary |
| `atms` | profile | channel | resilience | resilience | primary |
| `fdi_inflows` | profile | channel | resilience | resilience | |
| `inflation` | | control | | | |
| `interest_spread` | optional | optional | | | |
| `nonperforming_loans` | | | optional | optional | |
| `market_cap` | avoid | avoid | avoid | avoid | avoid |
| `IncomeGroup` | grouping | grouping | grouping | grouping | filter |
| `Region` | grouping | | | | |
