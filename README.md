<div align="center">
  <h1>📈 Financial Development & Economic Growth</h1>
  <p><strong>A Data Analytics Investigation into Global Financial Landscapes (2004–2021)</strong></p>

  <p>
    <a href="https://jupyter.org/"><img src="https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white" alt="Jupyter Notebook" /></a>
    <a href="https://www.python.org/"><img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" /></a>
    <a href="https://pandas.pydata.org/"><img src="https://img.shields.io/badge/Pandas-Data_Analysis-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="Pandas" /></a>
  </p>
</div>

<br />

## 📖 About This Project

This project investigates how **financial sector development** relates to **economic growth** across **216 countries** between 2004 and 2021, using data from the *World Bank World Development Indicators (WDI)*. 

The analysis is structured around a core research question and five sub-questions that together build a complete picture — from mapping the global financial landscape to stress-testing the finance-growth relationship through the **2008 financial crisis** and the **2020 COVID-19 shock**.

> **🎯 Target Audience**: Policymakers, development economists, and international financial institutions (e.g., IMF, World Bank).

---

## 👥 Team 2

**HWR Berlin, Introduction to Data Analytics, SoSe 2026**

- **Andrei Piatrouski**
- **Nikita Pasichnyk**
- **Hannes Gantzkow**
- **Jean Yves Nkwane Ebongue**

---

## 📊 Dataset Overview

| Metric | Details |
| :--- | :--- |
| **Source** | World Bank World Development Indicators (WDI) |
| **Panel Data** | 216 countries × 18 years (2004–2021) = **3,831 observations** |
| **Key Variables** | GDP growth, domestic credit, broad money, bank branches, ATMs, FDI inflows, inflation |
| **Metadata** | Region (7) and Income Group (4) per country |

---

## 📚 Project Documentation

The project is thoroughly documented through various Markdown files and notebooks:

- ❓ **[Business Questions](docs/business_questions.md)**: Stakeholder motivation, main research question, five sub-questions, and scope limits.
- 🗺️ **[Analysis Plan](docs/analysis_plan.md)**: Exact variables, filters, computations, and expected outputs for each sub-question.
- 📝 **[Data Notes](docs/notes_on_data.md)**: All six source files explained — structure, join logic, and variable coverage.
- 🧑‍💻 **AI Directories**: Documenting individual contributions and AI tool usage ([Andrei](AI_DIRECTORY_Andrei%20Piatrouski.md), [Jean Yves](AI_DIRECTORY_Jean%20Yves%20Nkwane.md), [Nikita](AI_DIRECTORY_Nikita_Pasichnyk.md), [Hannes](AI_DIRECTORY_Hannes_Gantzkow.md)).

---

## 🚀 Getting Started

### Prerequisites

Ensure you have Python installed, along with Jupyter Notebook or JupyterLab to view the analysis.

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

### Running the Analysis

1. Clone this repository.
2. Navigate to the `notebooks` directory.
3. Open the notebooks using Jupyter:
   ```bash
   jupyter notebook
   ```

---
<div align="center">
  <p>Made for <strong>HWR Berlin Data Analytics SoSe 2026</strong></p>
</div>

### 🔬 Key Econometric Indicators & Methodology

| Indicator | Source / Metric | Analytical Objective |
| :--- | :--- | :--- |
| **Financial Depth** | Domestic credit to private sector (% of GDP) | Measures financial intermediary scale and capital allocation |
| **Financial Breadth** | Broad money (M2/M3 as % of GDP) | Evaluates monetization and liquid liability availability |
| **Economic Growth** | Real GDP per capita growth (annual %) | Primary dependent variable tracking macroeconomic expansion |
| **Control Variables** | Inflation, Trade Openness, Government Expenditure | Controls for exogenous structural determinants |

### 🚀 Quick Start & Reproducibility

```bash
# 1. Clone repository
git clone https://github.com/Nejy190501/global-finance-data-analytics.git
cd global-finance-data-analytics

# 2. Setup virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\\Scripts\\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch Jupyter Notebook
jupyter notebook notebooks/
```
