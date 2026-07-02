# AI Directory — Nikita Pasichnyk (SQ1)

**Project:** Financial Development & Economic Growth
**Part:** Sub-Question 1 — The Financial Landscape (financial profiles & income-group validation)
**Notebook:** `notebooks/sq_1_Nikita/Data_Analysis_SQ1_Financial_Landscape.ipynb`

---

## Overview

| # | Tool | Where it was used | Prompt / Approach |
|---|------|-------------------|-------------------|
| 1 | Claude (Anthropic) | Sharpening the research question | *„I'm doing SQ1 of a finance–growth study on a World Bank WDI panel (216 countries, 2004–2021). Help me phrase, as an explanatory question, whether countries' financial-development profiles match the World Bank income classification — and explain why validating the income label matters before the other sub-questions group by it."* |
| 2 | Claude (Anthropic) | Notebook structure | *„Structure a Jupyter notebook for SQ1: build a clean country×year panel from the raw World Bank files, compute a median financial profile per income group, then run an exploratory k-means clustering to test whether the data reproduces the income groups, ending with a cross-tab and an outlier analysis. Give me the section order with short markdown headings."* |
| 3 | Claude (Anthropic) | Building the panel (setup cell) | *„From DataSheet_Main, DataSheet_GDP and country metadata (all with 4 header rows), filter my nine indicator codes, concatenate GDP, reshape wide→long→panel with melt and pivot, rename to readable column names, and merge IncomeGroup/Region on Country Code so I get one row per country-year. Show the cleanest pandas chain and explain melt vs. pivot."* |
| 4 | Claude (Anthropic) | Clustering methodology | *„I cluster countries on five financial indicators with k-means (k=4) to test against the four income groups. Help me justify two choices for the Q&A: why I standardize the features first, and why k=4. Also explain why dropping countries missing more than two of five indicators is honest but introduces a bias I must disclose."* |
| 5 | Claude (Anthropic) | Visualizations | *„Help me design four publication-quality matplotlib figures for SQ1: a median-profile heatmap by income group, a horizontal 'depth ladder' of cluster means, a cluster×income cross-tab heatmap, and an FDI-vs-credit outlier scatter. Keep a consistent palette, and tell me honestly where a per-column colour scale could mislead a reader."* |
| 6 | Claude (Anthropic) | Data-integrity check | *„Before I present, reproduce my SQ1 numbers from the raw CSVs and tell me if any are wrong: the median profiles per income group, the number of countries entering the clustering, the cluster means, and which countries are the outliers. Every figure on my slides has to be defensible."* |
| 7 | Claude (Anthropic) | speaker notes & presentation | *„Write speaker notes for a six-slide deck that I can deliver in 6 minutes, which is attached."* |
| 8 | DeepSeek | Cross-checking methodology (second opinion) | *„Independent check: I clustered 186 countries on five standardized financial indicators with k-means at k=4. Is k=4 defensible, or do an elbow / silhouette analysis and a comparison with k=3 and k=5 suggest a different number? Reason through whether a 'depth ladder' or four discrete tiers better describes the result."* |
| 9 | DeepSeek | Validating the statistical interpretation | *„Cross-check my reading of a cluster×income cross-tab: cluster 1 (shallow) holds 23 low / 38 lower-mid / 22 upper-mid / 2 high-income countries, and the deep clusters contain no low-income countries while the shallow one holds two high-income ones. Is the conclusion 'wealth is necessary but not sufficient for financial depth' statistically warranted, and what would falsify it?"* |
| 10 | DeepSeek | Questioning figure integrity | *„Stress-test my figures for honesty: my heatmap scales colour within each column, so a flat FDI row (2.2–3.5% of GDP) still shows a strong gradient. Is that misleading, how should I caveat it in the talk, and are there other places where my visual encoding could overstate a pattern the underlying numbers don't support?"* |

---

## Notes

- **No code was copied 1:1.** AI-generated suggestions were adapted to the actual data format (column names, paths, indicator codes) and integrated into the existing notebook workflow.
- **Methodological decisions** (standardization before clustering, k = 4, median over mean, merge on `Country Code`) were made and justified independently for the Q&A.
- **All findings were reproduced independently from the raw data** — median profiles per income group, the 186 countries entering the clustering, the cluster means, and the outliers (Cyprus/Malta, Guyana/Trinidad and Tobago).
- **Two models were used deliberately for triangulation:** methodology and interpretation were independently cross-checked with a second model (DeepSeek) rather than taken at face value — in particular the choice of k = 4 and the honest labelling of the heatmap.
- **The central limitation of the analysis** (186 of ~215 countries; those dropped skew poorer) was not hidden but actively carried into the report, slides, and Q&A.
- **Interpretation and conclusions** were formulated independently and checked against the team's research question.
