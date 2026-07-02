# CONTRIBUTIONS — Nikita Pasichnyk

**Project:** Financial Development & Economic Growth — HWR Berlin, SoSe 2026
**Team 2:** Andrei Piatrouski · Nikita Pasichnyk · Hannes Gantzkow · Jean Yves Nkwane Ebongue

---

## Sub-question I owned

| Sub-Question | Topic |
|---|---|
| SQ1 | The Financial Landscape — financial-development profiles & validation of the World Bank income classification (clustering) |

---

## My Contributions (Nikita Pasichnyk)

### 1. SQ1 — Full Analysis (`notebooks/sq_1_Nikita/Data_Analysis_SQ1_Financial_Landscape.ipynb`)

- **Built the analysis panel** from the raw World Bank files: loaded `DataSheet_Main`, `DataSheet_GDP` and country metadata (all with 4 header rows), filtered the nine financial indicator codes, concatenated GDP into the indicator set, reshaped wide → long → panel (melt + pivot), and merged Region/IncomeGroup on `Country Code` — producing one clean `country × year` table.
- **Part 1 — median financial profile** per income group across the five best-covered indicators (domestic credit, broad money, bank branches, ATMs, FDI), establishing that banking depth rises with income while FDI stays flat.
- **Part 2 — exploratory k-means clustering** (k = 4, standardized features) testing whether the data reproduces the four income groups; relabelled clusters by depth and cross-tabulated them against the income labels.
- **Key finding:** the data forms a *ladder of financial depth*, not four discrete income tiers — the cross-tab drifts but is not diagonal, with upper-middle income splitting across three clusters.

### 2. Data pipeline & first-look scripts

- Authored the three step-by-step Python scripts the notebook is built on: `sq1_erster_blick.py` (inspect the raw files), `sq1_panel_bauen.py` (build and save `panel_sauber.csv`), and `sq1_clustering.py` (clustering, cross-tab, outliers).

### 3. Four publication-quality visualizations

- Designed and coded the SQ1 figures in matplotlib with a consistent palette: a median-profile heatmap, a horizontal "depth ladder" of cluster means, a cluster × income cross-tab heatmap, and an FDI-vs-credit outlier scatter — each with honest annotations (e.g. flagging that the heatmap's per-column colour scale can overstate the flat FDI row).

### 4. Outlier analysis

- Identified and interpreted the cases where the income label and financial depth diverge: **Cyprus & Malta** as a two-country micro-cluster (FDI ≈ 90–100% of GDP), and **Guyana & Trinidad and Tobago** as high-income but among the shallowest financial systems — establishing the asymmetry that financial depth can lag income but never leads it.

### 5. Data-integrity verification

- Reproduced every reported figure directly from the raw CSVs — the median profiles per income group, the 186 countries entering the clustering, the cluster means, and the named outliers — so that every number presented is defensible under questioning.

### 6. Methodological choices

- Documented and justified the analytical decisions: **standardization** before clustering (the indicators are on very different scales), **k = 4** (to test directly against the four income groups), **median over mean** (robustness to financial-hub outliers), and the **Country-Code merge** (canonical matching). Disclosed the data-coverage drop (186 of ~215 countries, skewing toward better-covered economies) as an honest limitation.

### 7. Presentation — Section: The Financial Landscape (SQ1)

- Built the six-slide SQ1 deck following the problem → data → analysis → insight → implications arc, with embedded speaker notes sized for an 8–9-minute delivery; covers the profiles, the depth-ladder finding, the smeared diagonal, the outliers, and the implications for the team's main question.

---

## Tools & Technologies

| Category | Tools |
|---|---|
| Language | Python 3 |
| Data manipulation | pandas, NumPy |
| Visualization | matplotlib |
| Statistics / ML | scikit-learn (KMeans, StandardScaler) |
| Environment | Jupyter Notebook, PyCharm |
| Version control | Git / GitHub |
| AI assistance | Claude (Anthropic), DeepSeek (see `AI_DIRECTORY`) |

---

## Files I Authored or Modified

| File | Role |
|---|---|
| `notebooks/sq_1_Nikita/Data_Analysis_SQ1_Financial_Landscape.ipynb` | Full SQ1 analysis |
| `notebooks/sq_1_Nikita/charts/*.png` | Four SQ1 figures |
| `docs/script_nikita.md` | speaker notes |
| `AI_DIRECTORY_Nikita_Pasichnyk.md` | AI tool usage documentation |
| `CONTRIBUTIONS-Nikita_Pasichnyk.md` | This file |