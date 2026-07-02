# AI Directory — Andrei Piatrouski (SQ3 & SQ4)

**Projekt:** Financial Development & Economic Growth
**Teil:** Sub-Question 3 — 2008 Global Financial Crisis · Sub-Question 4 — 2020 COVID-19 Shock
**Notebook:** `notebooks/andrei_sq_3_4/sq3_sq4.ipynb`

---

## Übersicht

| # | Tool | Wo wurde benutzt | Prompt / Herangehensweise |
|---|------|-------------------|---------------------------|
| 1 | Claude (Anthropic) | Forschungsfragen schärfen | *„I have a World Bank WDI panel (216 countries, 2004–2021) with GDP growth and financial indicators. Help me phrase two explanatory sub-questions: one on how the 2008 financial crisis changed the finance–growth relationship, and one comparing the 2020 COVID shock and its recovery against 2008."* |
| 2 | Claude (Anthropic) | Notebook-Struktur | *„Structure a Jupyter notebook for a two-crisis analysis: build a clean country×year panel, plot GDP-growth trajectories by income group around each crisis, compute a per-country drop metric, compare who got hit hardest vs. who stayed resilient, then a regression linking 2008 profiles to the 2020 shock. Give me the section order with short markdown headings."* |
| 3 | Claude (Anthropic) | Panel bauen (Setup-Zelle) | *„From three World Bank CSVs (Main, GDP, country metadata, all with 4 header rows), select my indicator codes, concatenate GDP into the indicator set, then reshape wide→long→panel with melt and pivot, and merge Region/IncomeGroup so I get one row per country-year. Show the cleanest pandas chain."* |
| 4 | Claude (Anthropic) | Trajektorien-Liniencharts | *„Plot mean GDP growth by income group over 2004–2014 (and a second chart for 2017–2021) as line charts with markers, one coloured line per income group, a shaded span over the crisis years, a dashed zero line, and a clear legend."* |
| 5 | Claude (Anthropic) | Deviation-Score Methode | *„I want a per-country crisis metric that neutralises country size: crisis-year growth minus that country's own pre-crisis average growth. Help me compute this per country with groupby and explain why comparing each country against itself is fairer than a raw cross-country average."* |
| 6 | Claude (Anthropic) | „Who got hit" Balkendiagramme | *„Build a horizontal bar chart of the per-country drop for a curated set of countries — high-income (USA, Germany) and low-income (Tanzania, Ethiopia, Uganda) — plus resilient vs. hardest-hit groups, with labels outside the bars. Fix the long bars getting clipped and place the legend so it doesn't overlap the data."* |
| 7 | Claude (Anthropic) | Lineare Regression | *„Using scikit-learn, train a LinearRegression on countries' 2008 financial profiles to predict their 2008 growth drop, then test it frozen on the 2020 drop for the common countries. Report in-sample R², out-of-sample R², 5-fold cross-validation and MAE, and tell me honestly about multicollinearity and the small common sample."* |
| 8 | Claude (Anthropic) | R² / Methodik erklären | *„Explain R² in plain terms and what it means that my in-sample R² is positive but the out-of-sample R² is negative. Is my analysis methodologically sound and how do I defend it against an overfitting critique?"* |
| 9 | Claude (Anthropic) | Präsentations-Folien (SQ3/SQ4 + Fazit) | *„Review and upgrade my crisis slides for a university presentation: a '2008 timeline' intro, the trajectory and who-got-hit slides, the regression punchline, the same structure for COVID, and closing slides on crisis-free time / crisis time / overall takeaways."* (Ergebnis: `docs/script_andrei.md`) |
| 10 | Claude (Anthropic) | Code-Guide & Master-Notebook | *„Walk through my notebook cell by cell in plain language for someone with an SQL background"* (Ergebnis: `docs/andrei_code_guide.md`) und *„Combine all four team members' notebooks into one master notebook in presentation order, fix the data paths, and verify it runs end-to-end."* (Ergebnis: `notebooks/master_notebook.ipynb`) |

---

## Anmerkungen

- **Kein Code wurde 1:1 übernommen.** AI-generierte Vorschläge wurden an das konkrete Datenformat (Spaltennamen, Pfade, Indikator-Codes) angepasst und in den bestehenden Notebook-Workflow integriert.
- **Die Deviation-Score-Methode** (Vergleich jedes Landes mit seinem eigenen Vor-Krisen-Durchschnitt) sowie die kuratierten Länderbeispiele wurden inhaltlich selbst festgelegt und geprüft.
- **Die Regression** wurde eigenständig validiert: Ich habe die Werte reproduziert (In-/Out-of-Sample R², Cross-Validation, MAE) und die Grenzen (Multikollinearität, kleine gemeinsame Stichprobe n ≈ 89) bewusst in die Bewertung aufgenommen.
- **Interpretation und Schlussfolgerungen** wurden eigenständig formuliert und mit der Forschungsfrage des Teams abgeglichen.
