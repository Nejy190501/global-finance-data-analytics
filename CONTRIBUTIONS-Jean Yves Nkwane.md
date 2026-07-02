# CONTRIBUTIONS — Jean Yves Nkwane Ebongue

**Project:** Financial Development & Economic Growth — HWR Berlin, SoSe 2026  
**Team 2:** Andrei Piatrouski · Nikita Pasichnyk · Hannes Gantzkow · Jean Yves Nkwane Ebongue

---

## Project Overview

This project investigates how financial sector development relates to economic growth across 216 countries (2004–2021), using World Bank World Development Indicators. It is structured around one central research question and five sub-questions.

| Sub-Question | Topic | Owner |
|---|---|---|
| SQ1 | The Financial Landscape — clustering & income-group validation | Nikita |
| SQ2 | Which financial indicator correlates most with GDP growth in normal times? | Hannes |
| SQ3 | Impact of the 2008 Global Financial Crisis | Andrei |
| SQ4 | Impact of the COVID-19 2020 Shock | Andrei |
| SQ5 | The Shift in Retail Financial Access (ATMs vs Bank Branches) | **Yves** |

---

## My Contributions (Jean Yves Nkwane Ebongue)

### 1. Sub-Question 5 — Full Analysis (`notebooks/yves_sq_5/sq5.ipynb`)

Designed and implemented the complete SQ5 analysis, answering: *"How has the balance between physical bank branches and ATM networks evolved across income groups between 2004 and 2021, and to what degree does the speed of this shift track GDP growth performance in lower- and lower-middle-income economies?"*

**What I did:**

- **Data preparation:** Loaded the shared pre-processed panel, built a Country Name → Country Code lookup from the raw DataSheet, merged with country metadata to obtain IncomeGroup and Region for each observation.
- **Data quality check:** Calculated and reported missing value percentages per income group for the three key SQ5 variables (ATMs, bank branches, GDP growth).
- **Time series analysis (Charts 2a & 2b):**
  - Plotted the evolution of median bank branches and ATMs per 100k adults by income group (2004–2021), showing that branches plateaued while ATMs surged.
  - Created the ATM-to-Branch Ratio chart with custom colors per income group and a parity reference line, revealing a clear structural divergence.
- **Substitution speed metric:** Designed and coded a per-country metric measuring how fast ATMs replaced branches (ATM change minus branch change over the observation window), filtering for countries with ≥ 5 years of complete data. Identified the top 15 fastest-substituting countries among low- and lower-middle-income economies.
- **Correlation analysis (Chart 4):** Scatter plot of substitution speed vs. mean GDP growth with seaborn regression line. Computed and reported the Pearson correlation coefficient (r ≈ 0.39, p < 0.05), confirming a moderate positive association.
- **Written summary & policy implications:** Authored the final markdown cell with key findings, Q&A format, and actionable policy recommendations for the IMF/World Bank.

### 2. Presentation — Section 6: How Banking Access is Shifting

Prepared and presented the SQ5 slides in the team PowerPoint (`Team 2_ Financial Indicators per Country.pptx`), covering:
- The structural divergence between branches and ATMs
- Country-level substitution rankings
- The correlation with GDP growth
- Policy takeaways for low-income economies

### 3. Code Documentation & Quality

- Added detailed English comments to every code cell in `sq5.ipynb`, explaining:
  - What each step does and why
  - Data flow and merge logic
  - Statistical methods used
  - How to interpret each chart
- Fixed a bug where `colors_income` was used but never defined, which would have caused a runtime crash.

### 4. Project Infrastructure

- Contributed to the `extract_pptx.py` script for extracting text content from the team's PowerPoint presentation into a searchable text file.
- Created the AI Directory (`AI_DIRECTORY.md`) documenting AI tool usage for my part of the project.
- Created this CONTRIBUTIONS file.

---

## Tools & Technologies Used

| Category | Tools |
|---|---|
| Language | Python 3 |
| Data manipulation | pandas, NumPy |
| Visualization | matplotlib, seaborn |
| Statistics | scipy.stats (Pearson correlation) |
| Environment | Jupyter Notebook |
| Version control | Git / GitHub |
| AI assistance | ChatGPT (GPT-4), GitHub Copilot (see `AI_DIRECTORY.md`) |

---

## Files I Authored or Modified

| File | Role |
|---|---|
| `notebooks/yves_sq_5/sq5.ipynb` | Full SQ5 analysis — authored |
| `extract_pptx.py` | PPTX text extraction utility — contributed |
| `AI_DIRECTORY.md` | AI tool usage documentation — authored |
| `CONTRIBUTIONS.md` | This file — authored |
