# CONTRIBUTIONS — Andrei Piatrouski

**Project:** Financial Development & Economic Growth — HWR Berlin, SoSe 2026
**Team 2:** Andrei Piatrouski · Nikita Pasichnyk · Hannes Gantzkow · Jean Yves Nkwane Ebongue

---

## Sub-questions I owned

| Sub-Question | Topic |
|---|---|
| SQ3 | Impact of the 2008 Global Financial Crisis |
| SQ4 | Impact of the 2020 COVID-19 Shock & comparative recovery |

---

## My Contributions (Andrei Piatrouski)

### 1. SQ3 & SQ4 — Full Analysis (`notebooks/andrei_sq_3_4/sq3_sq4.ipynb`)

- **Built the analysis panel** from the raw World Bank files: loaded `DataSheet_Main`, `DataSheet_GDP` and country metadata, concatenated GDP into the indicator set, reshaped wide → long → panel (melt + pivot), and merged region/income-group labels — producing one clean `country × year` table for both crises.
- **GDP-growth trajectories** by income group as line charts (2004–2014 for 2008, 2017–2021 for COVID), with the crisis window shaded.
- **Deviation-score method:** designed a per-country metric — `crisis-year growth − own pre-crisis average` — so each country is compared against itself, neutralising size and weighting effects.
- **"Who got hit" horizontal bar charts** for both crises, with high-income (USA, Germany) and low-income (Tanzania, Ethiopia, Uganda) examples, plus resilient vs. hardest-hit countries.
- **Linear regression** (scikit-learn) testing whether 2008 financial profiles predict the COVID shock: trained on 2008, tested frozen on 2020; reported in-sample and out-of-sample R², 5-fold cross-validation and MAE, and was explicit about multicollinearity and the small common sample (n ≈ 89).

### 2. Presentation — Crisis sections & conclusion (`docs/script_andrei.md`)

- Authored the SQ3/SQ4 slides: a "What was 2008?" timeline, crisis trajectories, who-got-hit, and the regression punchline; mirrored the structure for COVID; wrote the closing "crisis-free time / crisis time / overall takeaways" slides.

### 3. Documentation & team integration

- **Code guide** (`docs/andrei_code_guide.md`): a plain-language, cell-by-cell walkthrough of my notebook.
- **Master notebook** (`notebooks/master_notebook.ipynb`): combined all four members' analyses into one notebook in presentation order (SQ1 → SQ2 → SQ5 → SQ3/SQ4), fixed the data paths so it runs top to bottom, and verified it executes end-to-end without errors.
- This `CONTRIBUTIONS` file.

---

## Tools & Technologies

| Category | Tools |
|---|---|
| Language | Python 3 |
| Data manipulation | pandas, NumPy |
| Visualization | matplotlib |
| Statistics / ML | scikit-learn (LinearRegression, cross-validation) |
| Environment | Jupyter Notebook |
| Version control | Git / GitHub |

---

## Files I Authored or Modified

| File | Role |
|---|---|
| `notebooks/andrei_sq_3_4/sq3_sq4.ipynb` | Full SQ3 & SQ4 analysis — authored |
| `notebooks/master_notebook.ipynb` | Combined team notebook — authored |
| `docs/script_andrei.md` | Crisis & conclusion presentation slides — authored |
| `docs/andrei_code_guide.md` | Cell-by-cell code guide — authored |
| `CONTRIBUTIONS-Andrei Piatrouski.md` | This file — authored |
