# Andrei — Code Guide (sq3_sq4.ipynb)

A plain-language, cell-by-cell walkthrough of the 2008 & COVID crisis notebook.
Built as I go through the code.

---

## Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from pathlib import Path
```

- **pandas (`pd`)** — the main tool for data manipulation and analysis (tables = DataFrames).
- **numpy (`np`)** — general numerical-array math. *Imported by convention but not actually used in this notebook.* The regression is done with scikit-learn, not numpy. (Good to know if asked: "where do you use numpy?" → honestly, here we don't; pandas + scikit-learn do the work.)
- **matplotlib.pyplot (`plt`)** — plotting / visualising the data.
- **pathlib.Path** — builds file paths that work on any operating system (Windows/Mac/Linux) without worrying about `/` vs `\`.

---

## Cell 1 — Setup (builds `df_panel`)

Everything downstream depends on this cell. It turns 3 raw World Bank files into one clean table.

### Load the data
```python
DATA_DIR = Path("../../data")
df_main      = pd.read_csv(DATA_DIR / "DataSheet_Main.csv", skiprows=4)
df_gdp       = pd.read_csv(DATA_DIR / "DataSheet_GDP.csv",  skiprows=4)
country_meta = pd.read_csv(DATA_DIR / "Metadata_Country.csv").dropna(axis=1, how="all")
```
- `DATA_DIR` = relative path to the `data/` folder (two levels up from the notebook).
- `skiprows=4` — World Bank CSVs start with **4 intro/metadata rows** before the real table header. We skip them so pandas reads the correct column names.
- `.dropna(axis=1, how="all")` on the metadata — drops any column that is completely empty.

### Pick the indicators
```python
selected_codes = [ "FS.AST.PRVT.GD.ZS", ... ]   # 9 financial indicator codes
years   = [str(y) for y in range(2004, 2022)]    # ["2004", ..., "2021"]
id_cols = ["Country Name", "Indicator Name"]
```
- `selected_codes` — the World Bank codes for the 9 financial indicators we want (credit, ATMs, FDI, etc.). They're stored by code in the dataset, so we filter by code.
- `years` — a **list comprehension** (compact for-loop) that builds the year column names as **strings**, because the columns are literally named "2004", "2005"…
- `id_cols` — the two descriptive columns that identify each row.

### Drop the non-country aggregate rows
```python
region_codes = country_meta[
    country_meta["Region"].isna() & country_meta["IncomeGroup"].isna()
]["Country Code"].tolist()
```
⚠️ **Key point:** this finds rows where **both Region and IncomeGroup are EMPTY**. Those are the World Bank **aggregate rows** ("World", "Euro area", "Sub-Saharan Africa", "Low income"…) — not real countries. We collect their codes so we can **remove them** in the next step, keeping only actual countries.

### Filter, then combine GDP with the financial indicators
```python
df_f     = df_main[df_main["Indicator Code"].isin(selected_codes)]
df_f     = df_f[~df_f["Country Code"].isin(region_codes)][id_cols + years]
df_gdp_f = df_gdp[df_gdp["Indicator Code"] == "NY.GDP.MKTP.KD.ZG"]
df_gdp_f = df_gdp_f[~df_gdp_f["Country Code"].isin(region_codes)][id_cols + years]
combined = pd.concat([df_f, df_gdp_f], ignore_index=True)
```
- Keep only the rows whose indicator is one of our 9 (`.isin(selected_codes)`).
- `~ ... .isin(region_codes)` — the `~` means **NOT**: drop the aggregate rows.
- GDP growth lives in a separate file, so we pull it out and **stack it on top** with `pd.concat` (same column shape, just more rows).

### Reshape: wide → long → panel
```python
df_long = combined.melt(id_vars=id_cols, value_vars=years, var_name="Year", value_name="Value")
df_long["Year"]  = df_long["Year"].astype(int)
df_long["Value"] = pd.to_numeric(df_long["Value"], errors="coerce")

df_panel = df_long.pivot_table(index=["Country Name", "Year"],
                               columns="Indicator Name", values="Value").reset_index()
```
- **melt** — turns the year *columns* into *rows*. Goes from "wide" (one column per year) to "long" (one row per country–indicator–year).
- `astype(int)` — years become numbers; `pd.to_numeric(..., errors="coerce")` — values become numbers, and anything non-numeric becomes `NaN` (missing) instead of crashing.
- **pivot_table** — spreads the indicators back out into columns, giving the final **panel**: one row = one country × one year, one column per indicator.

### Rename + attach income group
```python
df_panel = df_panel.rename(columns={ "GDP growth (annual %)": "gdp_growth", ... })
df_panel = df_panel.merge(country_meta[["TableName", "IncomeGroup", "Region"]],
                          left_on="Country Name", right_on="TableName", how="left").drop(columns="TableName")
```
- Renames the long official indicator names to short code-friendly ones (`gdp_growth`, `domestic_credit`, …).
- **merge** — joins each country to its `IncomeGroup` and `Region` label (a `left` join keeps every panel row even if no metadata match).

### Set up grouping + constants
```python
INCOME_ORDER = ["Low income", "Lower middle income", "Upper middle income", "High income"]
df_panel["IncomeGroup"] = pd.Categorical(df_panel["IncomeGroup"], categories=INCOME_ORDER, ordered=True)
COLORS   = { ... }   # one colour per income group, used in every chart
CHANNELS = ["domestic_credit", "broad_money", "bank_branches", "atms", "fdi_inflows"]
```
- `pd.Categorical(..., ordered=True)` — makes income groups sort low→high (not alphabetically) so charts read in a sensible order.
- `COLORS` — fixed colour per income group for consistent visuals.
- `CHANNELS` — the **5 financial indicators** used as the regression's input features later.

**Result:** `df_panel` = **3831 rows × 14 columns** — one row per country-year, with the financial indicators, GDP growth, income group, and region. This is the foundation for every later cell.

---

## For SQL people — concat vs merge

| pandas | SQL | direction |
|---|---|---|
| `pd.concat([a, b])` | `UNION ALL` | **vertical** — adds rows (no dedup) |
| `a.merge(b, on=...)` | `JOIN` | **horizontal** — adds columns |

`ignore_index=True` just renumbers the row index after stacking — nothing to do with columns.

### The reshape sequence (this trips people up)

The `concat` (UNION) happens **before** the country×year reshape, while both tables are still the same wide shape — so there are no "different columns" to worry about.

| Step | Format | Columns look like |
|---|---|---|
| 1. `df_f`, `df_gdp_f` | **wide** | `Country │ Indicator │ 2004 … 2021` |
| 2. **`concat`** (UNION ALL) | **wide** | same — GDP rows stacked under the 9 indicator rows |
| 3. **`melt`** | **long** | `Country │ Indicator │ Year │ Value` (one row per country-indicator-year) |
| 4. **`pivot_table`** | **panel** | `Country │ Year │ domestic_credit │ gdp_growth │ atms …` |

So "country+year as a row, indicators as columns" is the **final** state (step 4). The union is step 2, while indicators are still rows and years still columns. The metadata merge comes after step 4.

---

## What is `country_meta`?

It's the **lookup / dimension table** loaded from `Metadata_Country.csv` — one row per country:

```
Country Code │ TableName (= country name) │ Region │ IncomeGroup
ABW          │ Aruba                      │ Latin America & Caribbean │ High income
```

In SQL terms it's a small **dimension table** you join onto the big "fact" table (`df_panel`). It does two jobs in the setup cell:

1. **Identify the non-country aggregate rows** — rows where `Region` and `IncomeGroup` are both empty are World Bank totals ("World", "Euro area", "Low income"…). We grab their codes (`region_codes`) to filter them out.
2. **Label every country** — we `merge` (LEFT JOIN) its `IncomeGroup` and `Region` onto each country-year row, so later we can group by income tier.

> Note: it's a **single snapshot** — there's no year column, so a country gets *today's* income label applied across all 18 years. China, for example, actually moved up tiers during 2004–2021. Fine as a structural grouping, but worth flagging as a simplification. The join key is `Country Name = TableName`.

---

## Cell 2 — SQ3 line chart (GDP growth by income group, 2004–2014)

```python
df_08 = df_panel[df_panel["Year"].between(2004, 2014)].copy()
line_data = df_08.groupby(["IncomeGroup", "Year"])["gdp_growth"].mean().reset_index()
```

- **Slice to the 2008 window** (2004–2014) so the chart shows pre-crisis → crisis → recovery.
- **Why `.copy()`?** The slice is a *view* into `df_panel`. The next cell adds a `period` column to `df_08`; without `.copy()` pandas warns (`SettingWithCopyWarning`) and you risk mutating the master table. `.copy()` makes `df_08` independent. (Defensive — needed because a column is added later.)
- **groupby + mean** — for each `(IncomeGroup, Year)` it averages the GDP growth rate across the countries in that group. The chart then plots one line per income group with a grey band over the 2008–2009 crisis years.

### Methodology note — it's an *unweighted* mean (defensible, but know the trade-off)

`.mean()` here is a **simple average of growth rates** — every country counts equally (Luxembourg = China = Tuvalu). It is **not** GDP-weighted.

| Measure | Answers | Risk |
|---|---|---|
| **Unweighted mean** (what we use) | "How did the *typical country* in this group fare?" | Small, volatile economies can pull the average around |
| **GDP-weighted mean** | "How did the group's *total output* fare?" | Dominated by giants — high-income line ≈ the US |

**Why unweighted is the right choice here:** the whole study's unit of analysis is the *country* (deviation scores and the regression are all per-country vs each country's own trend). An unweighted line is consistent with that, and a weighted line would collapse "high-income fell hardest" into "the US fell." Grouping by income tier also keeps similar-sized economies together, reducing distortion.

**Limitation to state out loud:** small volatile economies add noise to the group mean — which is exactly why the next step normalizes each country against *its own* baseline (the deviation score). Averaging *rates* cross-sectionally is statistically standard; you'd only need absolute GDP levels for the "total output loss" view.

### How the matplotlib charts work (applies to every chart cell)

```python
fig, ax = plt.subplots(figsize=(11, 5))
```
- `plt.subplots()` returns **two objects**:
  - **`fig`** = the whole figure / canvas (the sheet of paper).
  - **`ax`** = the **axes** = the plot area inside it, with the x/y coordinate system. You draw on `ax`.
- **`figsize=(11, 5)`** = canvas size in **inches** (width 11, height 5). *This* sets how big the chart is.

Common `ax` methods used across the notebook:
| Call | What it does |
|---|---|
| `ax.plot(x, y, ...)` | draws a line (markers = the dots) |
| `ax.bar(...)` / `ax.barh(...)` | vertical / horizontal bars |
| `ax.set_title / set_xlabel / set_ylabel` | text labels |
| `ax.set_xticks(range(2004, 2015))` | which **tick marks** show on the x-axis (one per year 2004–2014). *Not* the size. `range` excludes the end, so 2015 → last tick is 2014. |
| `ax.axvspan(2008, 2009, ...)` | shaded vertical **band** (the crisis years) |
| `ax.axhline(0, ...)` | horizontal reference line at y = 0 |
| `ax.legend(...)` | the colour key |
| `ax.grid(...)` | faint gridlines |
| `plt.tight_layout()` | auto-adjusts padding so titles/labels/legend don't overlap or get clipped |
| `plt.show()` | renders the figure |

---

## Cell 3 — SQ3 deviation score (the heart of SQ3)

This is where we measure *who got hit hardest* — and the trick is to measure each country against **its own** pre-crisis trend.

### Tag each year with a period
```python
df_08["period"] = pd.cut(df_08["Year"], bins=[2003, 2007, 2009, 2014],
                         labels=["pre_crisis", "crisis", "recovery"])
```
`pd.cut` buckets each row's year. Bins are **right-inclusive** `(left, right]`:
- `(2003, 2007]` → **pre_crisis** (2004–2007)
- `(2007, 2009]` → **crisis** (2008–2009)
- `(2009, 2014]` → **recovery** (2010–2014)

### Build the two numbers per country
```python
pre_avg   = df_08[df_08["period"]=="pre_crisis"].groupby("Country Name")["gdp_growth"].mean()
crisis_09 = df_08[df_08["Year"]==2009].set_index("Country Name")["gdp_growth"]
```
- `pre_avg` — each country's **average growth 2004–2007** (its "normal pace").
- `crisis_09` — each country's growth in **2009 only**.

> ⚠️ Note: the drop uses the **single year 2009**, not the 2008–2009 average. So the `"crisis"`/`"recovery"` labels are defined but only `pre_crisis` is actually used. **Why 2009?** It's the trough — Lehman fell Sept 2008, but GDP dropped hardest in 2009.

### Combine and compute the drop
```python
deviation = pd.concat([pre_avg, crisis_09], axis=1).dropna()
deviation["drop"] = deviation["gdp_2009"] - deviation["pre_crisis_avg"]
```
- **`axis=1`** = join the two Series **side by side by country index** (like a JOIN → 2-column table). Contrast with the setup cell's `concat` which stacked rows; `axis=1` adds columns.
- `dropna()` — drop countries missing either value.
- **`drop = 2009 growth − own pre-crisis average`.** Negative = fell below its own trend; more negative = harder hit.

`len(deviation)` = **205 countries** scored. `.sort_values("drop").head(10)` = 10 hardest hit (Latvia −26.9, Estonia/Lithuania −23, …).

**Why this design matters (say it in the talk):** scoring each country against *its own* baseline — not zero, not other countries — neutralizes the size/weighting issue. A country that normally grows 10% falling to −14% (−24pp) is correctly ranked harder-hit than one going 2%→−4% (−6pp), even though −4% *looks* worse in raw terms.

### How `concat(axis=1)` keeps the join correct
Pandas aligns by **index label, not row position**. Both Series are indexed by country name, so Latvia always meets Latvia regardless of row order — like `JOIN ON country_name`. Default is an **outer join** (keep all, missing → `NaN`); the `.dropna()` then makes it effectively an **inner join** (only countries complete in both → 205). Safe because each index is **unique** (one row per country); duplicate labels would cause a messy cross-product.

---

## Cell 4 — SQ3 pre-crisis financial profile (resilient vs hardest hit)

Compares the *financial fingerprint* of survivors vs casualties, **before** the crisis.

### Pick the top/bottom 10%
```python
n = max(1, int(len(deviation) * 0.1))
resilient = deviation.nlargest(n, "drop").index    # fell the LEAST
hardest   = deviation.nsmallest(n, "drop").index    # fell the HARDEST
```
- `n` = 10% of countries: `int(205 * 0.1)` = 20. **`max(1, …)`** is a safety floor — guarantees at least 1 even on a tiny dataset (so you never request "the 0 largest"). Here n = 20.
- `drop` is **negative** for countries that fell, so:
  - **`nlargest`** → drop closest to 0 / positive → fell least → **resilient**
  - **`nsmallest`** → most negative drop → fell hardest → **hardest hit**
- `.index` → the country names of each group.

### Compute each group's median financial fingerprint
```python
pre_profiles = df_08[df_08["period"]=="pre_crisis"].groupby("Country Name")[CHANNELS].mean()
p_res = pre_profiles.loc[pre_profiles.index.isin(resilient)].median()
p_hit = pre_profiles.loc[pre_profiles.index.isin(hardest)].median()
```
- `pre_profiles` — each country's **average 2004–2007 value** for the 5 channels (credit, broad money, branches, ATMs, FDI) = its pre-crisis financial fingerprint.
- `p_res` / `p_hit` — the **median** of each channel across the resilient / hardest-hit group.
- **Why median, not mean?** Financial indicators are skewed (a few countries with huge credit/FDI); the median resists outliers and reflects the *typical* country.

The grouped bar chart shows, per channel, a green bar (resilient) next to a red bar (hardest hit).

**The point:** if the hardest-hit group had *higher* pre-crisis credit and FDI than the resilient group, that's direct evidence that **financial depth was the vulnerability** in 2008 — the core SQ3 finding.

---

## Cells 5–7 — SQ4 (COVID), a mirror of Cells 2–4

Same code, same logic — only the time window and trough year change. Using the *identical method twice* is what makes the two crises comparable.

| | SQ3 (2008) | SQ4 (COVID) |
|---|---|---|
| Window | `df_08` = 2004–2014 | `df_cov` = 2017–2021 |
| Period bins | `[2003, 2007, 2009, 2014]` | `[2016, 2019, 2020, 2021]` |
| Pre-crisis baseline | avg 2004–2007 | avg 2017–2019 |
| Trough year | **2009** | **2020** |
| `drop` = | `gdp_2009 − pre_crisis_avg` | `gdp_2020 − pre_covid_avg` |
| Countries scored | 205 | 210 |
| Variables | `deviation`, `pre_avg`, `crisis_09` | `deviation_cov`, `pre_avg_cov`, `shock_20` |

**The payoff — same method, opposite result:** in 2008 the hardest-hit group had *higher* pre-crisis credit/FDI (depth = vulnerability). In 2020 the financial fingerprints of hardest-hit vs resilient look **basically the same** — financial depth does *not* separate them. What separates them is tourism exposure, which isn't in the data at all. That contrast is the whole SQ3-vs-SQ4 story, and it only lands because the method is identical.

---

## Cell 9 — The regression (train on 2008, test on 2020)

The centerpiece. A linear model learns "financial profile → crash" from 2008, then we freeze it and try it on 2020.

### Step by step
1. **Build X (inputs).** `build_features` averages each country's 5 channels over the pre-crisis years → `X_2008` (2004–07 profile), `X_2020` (2017–19 profile). One row per country, 5 columns.
2. **Attach y (target = the drop).** `.join(deviation[["drop"]])` glues profile ↔ fall:
   - `train` = 2008 profile + 2009 drop
   - `test` = 2020 profile + 2020 drop
3. **Common countries.** `common = train.index.intersection(test.index)`, `.loc[common]` → **n = 89**.
4. **To arrays.** `.values` → `X_train` (89×5), `y_train` (89), and same for test (sklearn wants numpy arrays).
5. **Train on 2008.** `model.fit(X_train, y_train)` solves for the 5 weights (least squares).
6. **Predict** in-sample (2008) and out-of-sample (2020).
7. **Score.** R² **+0.16** (2008) vs **−0.16** (2020).

### What `fit` actually computes
With 5 inputs it's not a 2D line but a 5-D plane:
```
drop ≈ b0 + b1·credit + b2·broad_money + b3·branches + b4·atms + b5·fdi
```
`fit` finds the `b`'s that minimize the squared errors on the 89 countries. `predict` plugs a country's 5 numbers into that frozen formula.

### Why the intersection (NOT about leakage)
Train and test have **different targets** (2009 drop vs 2020 drop), so the model never sees 2020 outcomes during training — there's no leakage even for countries in both. The intersection is about a **fair comparison**: testing transfer on the *same 89 countries*, so the R² drop reflects *the crises differing*, not *different country samples*. Same patients, two diseases.

### Why the index is used everywhere
The index = **country name = the universal join key** (like a SQL primary key). Pandas auto-aligns on it:
| Operation | Index role |
|---|---|
| `groupby("Country Name")` | country becomes the index |
| `X_2008.join(deviation["drop"])` | matches profile↔drop by country |
| `train.index.intersection(test.index)` | countries in both |
| `train.loc[common]` | select those countries |

Country identity is baked into each row as its index, so every join/filter lines countries up correctly (Germany always meets Germany) without dragging a key column around manually.

### Coefficients
`model.coef_` = the `b` weights (FDI −0.238 is biggest: more pre-crisis FDI → bigger fall). **Caveat:** the 5 features are correlated (multicollinearity), so don't over-interpret individual coefficients — the trustworthy result is the **R² flip**, not any single weight.

---

## The remaining cells — pure visualisation (simpler)

From here it's mostly plotting, reusing the matplotlib basics above. Nothing new analytically.

### Cell 8 — Cross-crisis comparison (`both`)
```python
both = deviation[["drop"]].rename(columns={"drop":"drop_2008"}).join(
       deviation_cov[["drop"]].rename(columns={"drop":"drop_2020"}), how="inner")
```
- Joins the two deviation tables **by country index** → one row per country with `drop_2008` and `drop_2020`.
- Takes the bottom-10% (hardest hit) of *each* crisis, then computes each group's **average drop in both crises**.
- The 4-bar chart shows: the 2008-hardest-hit group barely moved in 2020, and the 2020-hardest-hit group barely moved in 2008 → **different victims**. Also prints the two country lists (only ~5 overlap).

### Cell 10 — Regression scatter (predicted vs actual)
- Two side-by-side scatter plots (2008 in-sample, 2020 out-of-sample).
- Each dot = a country: x = actual drop, y = predicted drop.
- The dashed `y = x` line = "perfect prediction." Dots near it = good; scattered/inverted = bad.
- R² is annotated on each panel. Visually: 2008 dots loosely follow the line (+0.16); 2020 dots scatter the wrong way (−0.16).

### Cells 11 & 12 — The slide bar charts (2008 and 2020 "Who Got Hit")
- Hand-picked named countries (`hit_countries` / `resilient_countries` dicts), looked up in `deviation` / `deviation_cov`.
- Horizontal bars of each country's deviation, colour-coded (red = hard hit, green = resilient), value labels, fixed x-limits so labels don't clip.
- Pure presentation visuals — same numbers as the deviation tables, just the selected countries. Saved as PNGs (`slide8_who_got_hit.png`, `slide12_who_got_hit_covid.png`) for the deck.
- Note: 2020's resilient set is `China, Egypt, Viet Nam, Bangladesh` (diversified, low-tourism economies that barely dipped).

---

*End of guide. Flow: Setup → SQ3 (trajectory → deviation → profile) → SQ4 (same, mirrored) → cross-crisis check → regression (the punchline) → slide charts.*