# AI Directory — Jean Yves Nkwane Ebongue (SQ5)

**Projekt:** Financial Development & Economic Growth  
**Teil:** Sub-Question 5 — The Shift in Retail Financial Access  
**Notebook:** `notebooks/yves_sq_5/sq5.ipynb`

---

## Übersicht

| # | Tool | Wo wurde benutzt | Prompt / Herangehensweise |
|---|------|-------------------|---------------------------|
| 1 | ChatGPT (GPT-4) | Forschungsfrage formulieren | *„I have World Bank WDI data on ATMs per 100k adults and bank branches per 100k adults for 216 countries (2004–2021), plus GDP growth. I want to investigate the structural shift from physical branches to ATMs. Help me formulate a clear, specific research question for this analysis that ties the shift to economic growth in lower-income economies."* |
| 2 | ChatGPT (GPT-4) | Notebook-Struktur & Markdown-Zellen | *„Structure a Jupyter notebook for the following analysis: (1) load a pre-processed panel CSV, (2) merge with country metadata to get income groups, (3) check data quality, (4) plot ATMs and branches over time by income group, (5) compute an ATM-to-branch ratio, (6) identify countries with the fastest substitution, (7) test correlation with GDP growth. Give me section headings with short explanations I can use as markdown cells."* |
| 3 | ChatGPT (GPT-4) | Daten-Merge-Logik (Cell 5) | *„My panel DataFrame uses 'Country Name' but the metadata CSV uses 'Country Code'. How do I bridge the two? I have access to the raw DataSheet_Main.csv which has both columns. Show me the cleanest pandas merge chain to add IncomeGroup and Region to my panel."* |
| 4 | ChatGPT (GPT-4) | Datenqualitätsprüfung (Cell 6) | *„Write a compact loop in pandas that prints the percentage of missing values per income group for a list of variables. I want to check ['atms', 'bank_branches', 'gdp_growth'] grouped by 'IncomeGroup'."* |
| 5 | ChatGPT (GPT-4) | Zeitreihen-Plot (Cell 8) | *„Create a matplotlib subplot with 2 panels side by side: left = median bank branches per 100k by income group over time, right = median ATMs per 100k by income group over time. Use markers, a legend, and a suptitle. Data is in a DataFrame with columns IncomeGroup, Year, bank_branches, atms."* |
| 6 | ChatGPT (GPT-4) | ATM-to-Branch Ratio Plot (Cell 9) | *„I want to plot the ATM-to-branch ratio over time per income group. Add a horizontal dashed line at ratio=1 labeled 'Parity'. Use a dictionary of custom colors per income group and square markers. Make it publication-ready with tight_layout."* |
| 7 | ChatGPT (GPT-4) | Substitution-Speed Berechnung (Cell 11) | *„For each country in the 'Low income' and 'Lower middle income' groups, I want to compute: (1) ATM change = last year ATMs minus first year ATMs, (2) branch change = same for branches, (3) substitution speed = ATM change minus branch change, (4) mean GDP growth. Only include countries with at least 5 years of complete data. Write a groupby-apply function that returns a pd.Series with these metrics."* |
| 8 | ChatGPT (GPT-4) | Scatter Plot + Pearson-Korrelation (Cell 13) | *„Create a scatter plot of substitution_speed (x) vs gdp_growth_mean (y) using seaborn regplot. Then compute and print the Pearson correlation coefficient with its p-value using scipy.stats.pearsonr."* |
| 9 | ChatGPT (GPT-4) | Interpretation & Summary (Cell 14) | *„Based on these results, write an analytical summary in markdown: the ATM-to-branch ratio diverges by income group, branches have plateaued, ATMs surged in middle-income countries, and the Pearson r is approximately 0.39 with p < 0.05 for low-income economies. Frame it as policy advice for the IMF/World Bank."* |
| 10 | GitHub Copilot | Code-Vervollständigung | Inline-Autocomplete für repetitive matplotlib-Formatierung (`ax.set_title`, `ax.set_xlabel`, `ax.legend`, `plt.tight_layout`) und Pandas-Methodenketten (`.groupby().median()`, `.merge()`, `.dropna()`). Kein expliziter Prompt — kontextbasierte Vervollständigung. |

---

## Anmerkungen

- **Kein Code wurde 1:1 übernommen.** AI-generierte Vorschläge wurden an das konkrete Datenformat (Spaltennamen, Pfade, Datentypen) angepasst und in den bestehenden Notebook-Workflow integriert.
- **Plotting-Parameter** (Farben, Marker-Größen, Figsize) wurden manuell angepasst, um zum Team-Design zu passen.
- **Datenlogik** (Merge-Kette Panel → Lookup → Meta, Filterung auf Länder mit Region, Schwellenwert 5 Jahre) wurde inhaltlich geprüft und mit dem Rest des Projekts abgestimmt.
- **Interpretation** der Ergebnisse wurde eigenständig formuliert und mit der Forschungsfrage des Teams abgeglichen.
