# CONTRIBUTIONS — Hannes Gantzkow

**Project:** Financial Development & Economic Growth — HWR Berlin, SoSe 2026  
**Team 2:** Andrei Piatrouski · Nikita Pasichnyk · Hannes Gantzkow · Jean Yves Nkwane Ebongue

---

## Sub-question I owned

| Sub-Question | Topic |
|---|---|
| SQ2 | What Drives Growth in Normal Times — ranking financial indicators by their correlation with GDP growth across income groups |

---

## My Contributions (Hannes Gantzkow)

### 1. Initial data preparation and shared panel setup

- Contributed to the initial preparation of the World Bank dataset used throughout the project.
- Loaded and inspected the raw World Bank files, including the main financial indicator dataset, GDP growth data, country metadata, and indicator metadata.
- Cleaned the imported files by removing empty columns, checking missing values, and reviewing the available indicator structure.
- Helped build the shared country-year panel for the period 2004–2021, combining financial indicators with GDP growth into a consistent analysis-ready format.
- Supported the harmonization of country names and metadata so that income groups could be merged correctly into the analytical dataset.
- Ensured that the resulting panel could be reused across the project as a common basis for the sub-questions.

### 2. SQ2 — Full analysis of financial indicators and GDP growth in normal times

- Owned the analysis for SQ2: identifying which financial indicator correlates most strongly with GDP growth in normal, non-crisis years.
- Focused on five core financial indicators: domestic credit, broad money, bank branches, ATMs, and FDI inflows.
- Excluded the major crisis years 2008, 2009, 2020, and 2021 in order to separate regular finance-growth relationships from global shock effects.
- Calculated correlations between each financial indicator and GDP growth, both overall and separately by World Bank income group.
- Compared whether the ranking of financial indicators changes across low-income, lower-middle-income, upper-middle-income, and high-income countries.
- Produced summary tables showing the direction and strength of the relationships between financial indicators and GDP growth.

### 3. Income-group comparison

- Structured the SQ2 analysis around income groups to test whether financial indicators behave differently depending on a country’s stage of development.
- Used the World Bank income classification to compare patterns across low-income, lower-middle-income, upper-middle-income, and high-income economies.
- Paid particular attention to whether indicators related to financial access, such as bank branches and ATMs, matter differently from financial-depth indicators, such as domestic credit and broad money.
- Interpreted the results carefully, emphasizing that the analysis identifies correlations rather than causal effects.

### 4. Visualizations for SQ2

- Designed and coded the SQ2 visualizations in Python using pandas and matplotlib.
- Created charts showing the strongest absolute correlation between financial indicators and GDP growth by income group.
- Built trend plots to compare how selected indicators and GDP growth developed over time across income groups.
- Adjusted the visualizations for readability, including income-group ordering, consistent colors, axis labels, legends, and annotations.
- Revised the plots to avoid misleading continuity across crisis years by separating normal-year periods where appropriate.

### 5. Interpretation and limitations

- Developed the main interpretation of SQ2: the finance-growth relationship is not uniform across income groups.
- Highlighted that different financial channels appear relevant for different country groups, rather than one single indicator explaining growth everywhere.
- Included methodological limitations, especially the fact that correlation does not imply causation.
- Reflected on data-availability issues, missing values, and the sensitivity of results to the exclusion of crisis years.
- Positioned SQ2 as the “normal-times” benchmark for the later crisis analyses in SQ3 and SQ4.

### 6. Presentation — Section: What Drives Growth in Normal Times (SQ2)

- Prepared the SQ2 section of the presentation, including the transition from the financial landscape to the normal-times correlation analysis.
- Built the slide narrative around the question of which financial channels matter most before moving into crisis periods.
- Contributed speaker notes for the SQ2 slides, focusing on explaining the method, results, and limitations clearly.
- Revised slide wording and conclusions to make the findings more precise, balanced, and aligned with the course level.

---

## Tools & Technologies

| Category | Tools |
|---|---|
| Language | Python 3 |
| Data manipulation | pandas, NumPy |
| Visualization | matplotlib |
| Environment | Jupyter Notebook |
| Version control | Git / GitHub |
| AI assistance | ChatGPT 5.5 Thinking (see `AI_DIRECTORY`) |

---

## Files I Authored or Modified

| File | Role |
|---|---|
| `notebooks/data_overview_plus_merging.ipynb` | Initial data overview, cleaning, merging, and shared panel preparation |
| `notebooks/hannes_sq2/sq2.ipynb` | Full SQ2 analysis |
| `docs/script_hannes.md` | Speaker notes and presentation support for SQ2 |
| `AI_DIRECTORY_Hannes_Gantzkow.md` | AI tool usage documentation |
| `CONTRIBUTIONS-Hannes_Gantzkow.md` | This file |
