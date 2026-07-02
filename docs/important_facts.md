# Important Facts & Reference

> **Legend:**
> 🌍 = relevant for the whole team
> 🔬 = Andrei's working notes for SQ3 & SQ4 only

---

---

# 🌍 SHARED — Whole Team

---

## Variables — What They Mean and What Good Looks Like

### `gdp_growth` — GDP Growth (annual %)
The outcome variable. Measures how much the economy grew compared to the previous year in real terms (inflation adjusted).
- Global median: **3.4%**
- Negative = the economy shrank (recession)
- Low-income countries average higher growth (~4.7%) than high-income (~2.4%) — normal convergence, not a sign of better financial systems

### `domestic_credit` — Domestic Credit to Private Sector (% of GDP)
Bank loans given to businesses and households as a share of the total economy.
Mortgages, business loans, consumer credit — everything banks lend to the private sector.
- Low income median: **10.8%** — almost no lending reaching the private sector
- High income median: **81.8%** — banks lend roughly as much as the economy produces
- Above 100% (Germany, UK, Ireland) means total lending exceeds annual GDP

### `broad_money` — Broad Money / M2 (% of GDP)
Total size of the monetary system — all cash, bank deposits, savings accounts held by everyone.
- Low income median: **22.9%** / High income median: **77.5%**
- **Difference from domestic_credit:** domestic_credit = what banks lend OUT; broad_money = what people hold IN the financial system

### `bank_branches` — Commercial Bank Branches (per 100,000 adults)
How many physical bank offices exist relative to the adult population. Measures **access**, not depth.
- Low income median: **2.2** / High income median: **23.8**
- Declining in rich countries over 2004–2021 as digital banking replaces physical branches

### `atms` — ATMs (per 100,000 adults)
Physical cash machines per adult population. Earlier step toward digital access.
- Low income median: **1.8** / High income median: **67.6**

### `fdi_inflows` — Foreign Direct Investment, Net Inflows (% of GDP)
Long-term foreign capital entering the economy (factories, acquisitions, infrastructure).
- Global median: **3.0%**
- Negative = more capital leaving than entering

### `inflation` — Inflation, GDP Deflator (annual %)
How fast prices are rising. Used as a control variable.
- Global median: **3.5%** / High income median: **2.0%**
- Above ~10% weakens financial system effectiveness

### `interest_spread` — Interest Rate Spread (%)
Margin banks charge — lending rate minus deposit rate.
- Healthy: **2–4%** / High (>10%) = inefficient or risky banking
- ~43% missing data — supporting evidence only

### `nonperforming_loans` — NPL Ratio (% of total loans)
Share of loans where borrowers stopped repaying.
- Healthy: **<3%** / Crisis spike: can reach 20–30%
- ~52% missing data — supporting evidence only

### `market_cap` — Stock Market Capitalisation (% of GDP)
Size of stock market relative to the economy.
- ~65% missing, mostly in low/lower-middle countries
- **Avoid as primary variable**

---

## Income Groups — Basis and Benchmarks

Assigned by the World Bank based on **Gross National Income (GNI) per capita** in USD:

```
Low income           < $1,145
Lower middle income    $1,145 – $4,515
Upper middle income    $4,516 – $14,005
High income          > $14,005
```

**GNI = GDP + income earned by residents abroad − income sent out by foreigners**

| Income Group | Countries | GDP growth | domestic_credit | broad_money | bank_branches | atms |
|---|---|---|---|---|---|---|
| Low income | 26 | 4.7% | 10.8% | 22.9% | 2.2 | 1.8 |
| Lower middle | 54 | 4.4% | 23.7% | 40.3% | 6.4 | 11.1 |
| Upper middle | 54 | 3.5% | 41.6% | 49.4% | 13.2 | 36.1 |
| High income | 82 | 2.4% | 81.8% | 77.5% | 23.8 | 67.6 |

**Caveat:** Classification is a single 2024 snapshot applied across 18 years. Treat as structural category, not time-varying truth.

---

## Crisis Timeline — 2008

- **2006–2007** — US housing cracks, subprime defaults rise
- **Aug 2007** — BNP Paribas freezes funds; interbank lending seizes up
- **Mar 2008** — Bear Stearns collapses
- **Sep 15, 2008** — Lehman Brothers bankruptcy ($600bn); AIG bailed out; money markets break
- **Oct 2008** — Global markets crash, credit freezes
- **Q4 2008 – Q1 2009** — Real economy collapses in US, UK, Eurozone, Japan
- **2009** — First negative global GDP growth since WW2

**Key fact: 2008 annual data looks mild because the first 9 months were fine. 2009 is the real hit.**

---

## Crisis Timeline — 2020

- **Dec 2019** — First cases in Wuhan
- **Mar 11, 2020** — WHO declares pandemic
- **Mar 2020** — Global lockdowns; markets fall 34% in 33 days (fastest crash in history)
- **Q2 2020** — Sharpest single-quarter GDP collapse ever: US –9%, UK –20%, Eurozone –12%
- **2021** — Strong rebound as vaccines roll out

**Key fact: Unlike 2008, the full shock is captured in the 2020 annual number. 2021 is the recovery.**

---

## 2008 vs 2020 — Side by Side

| | 2008 | 2020 |
|---|---|---|
| Origin | Inside the financial system | External (health crisis) |
| Speed | Slow build, sudden collapse Sep 2008 | Near-instantaneous (weeks) |
| Annual data | 2008 mild, **2009 is the real hit** | **2020 shows the full hit** |
| Recovery | Slow, uneven, 4–5 years | Sharp V-shape, mostly done by 2021 |
| Who was hit hardest | Financially deep, globally integrated economies | Tourism-dependent and trade-open economies |

---

---

# 🔬 ANDREI — SQ3 & SQ4 Working Notes

---

## Hypotheses (Annahmen)

### SQ3 — 2008 Global Financial Crisis

**H3 (main):** During the 2008 crisis, deeper financial systems amplified the shock rather than absorbed it. Countries with higher pre-crisis domestic_credit and broad_money suffered larger contractions, and income group determined the severity.

- **H3a:** Pre-crisis domestic_credit correlates positively with the 2009 GDP drop — more lending depth meant a harder fall, especially in high-income economies.
- **H3b:** Low-income countries were relatively shielded because their financial systems were too shallow to be transmission channels of the crisis.
- **H3c:** Countries with higher pre-crisis FDI inflows recovered faster after 2009 because external capital returned more quickly to open, investor-trusted economies.

### SQ4 — COVID-19 Shock 2020

**H4 (main):** The 2020 shock hit all income groups more uniformly than 2008 because it originated outside the financial system. But financial depth accelerated the 2021 recovery — the reverse of the 2008 pattern.

- **H4a:** The 2020 GDP drop is weakly correlated with financial depth — unlike 2009, income group explains less of the variance in shock severity.
- **H4b:** Countries with higher broad_money going into 2020 rebounded faster in 2021 because their financial systems channeled government stimulus more effectively.
- **H4c:** The recovery speed ranking across income groups flipped between the two crises — high-income recovered faster after COVID but slower relative to low-income after 2008.

---

## Methodology — Deviation Score

```
drop = gdp_growth in crisis year − average gdp_growth in pre-crisis years

2008:  drop = gdp_2009 − mean(gdp_growth, 2004–2007)
2020:  drop = gdp_2020 − mean(gdp_growth, 2017–2019)
```

**Why not raw growth:** A country normally growing at +8% dropping to –14% (drop = –22) was hit far harder than one normally at +2% dropping to –2% (drop = –4), even though both went negative. Deviation measures the shock relative to each country's own baseline.

**Why no linear trend:** Only 4 pre-crisis years — too few for a reliable per-country trend. Simple mean is more honest.

**Resilient** = top 10% by drop score (smallest deviation from baseline)
**Hardest hit** = bottom 10% (largest negative deviation)

---

## Linear Regression — Train on 2008, Predict 2020

**Method:** Multiple linear regression (OLS — Ordinary Least Squares). OLS and linear regression are the same thing. OLS describes how the line is fitted — it minimizes the sum of squared distances between actual and predicted values.

**What we do:**
```
Train:   pre-crisis financial profile (2004–2007) → deviation score 2009
Predict: feed same model the pre-COVID profile (2017–2019) → predicted 2020 drop
Compare: predicted vs actual 2020 drops
```

**How to read the output:**

| Metric | What it means |
|---|---|
| R² on 2008 (in-sample) | How well financial profiles explain the 2008 drops |
| R² on 2020 (out-of-sample) | How well the 2008 model predicts 2020 — if this collapses, crises are different |
| MAE (mean absolute error) | Average prediction error in percentage points |
| Coefficients | Which financial indicator drove vulnerability most in 2008 |

**Actual results:**
- R² on 2008 (in-sample): **0.16** — weak but positive, financial profiles have some signal
- R² on 2020 (out-of-sample): **–0.16** — actively wrong, the model predicts the inverse of reality

**What negative R² means:**
A negative R² does not mean a code error. It means the model performs worse than simply predicting the mean for every country. The financial characteristics that predicted vulnerability in 2008 are inversely related to the 2020 shock — countries expected to fall (deep financial systems) held up, countries expected to be fine (shallow, tourism economies) collapsed.

**Honest limitation — this is a lightweight regression:**
Simple OLS, five input variables, no feature scaling, no cross-validation, small sample. In-sample R² of 0.16 means financial profiles alone explain only 16% of 2008 growth outcomes. This model is not built to be the best possible predictor — it is built to answer one question: does the 2008 vulnerability pattern transfer to 2020? The –0.16 answers that unambiguously.

**Why this is stronger than two separate regressions:**
Running two regressions separately just shows different coefficients. Training on 2008 and testing on 2020 uses the model as a genuine prediction tool — the prediction error itself is the evidence that the crises are structurally different.

---

## How to Read the Profile Bar Chart

Compares median pre-crisis financial fingerprint of resilient vs hardest-hit countries across: `domestic_credit`, `broad_money`, `bank_branches`, `atms`, `fdi_inflows`

| What you see | What it means |
|---|---|
| Red bar taller for `domestic_credit` | H3a confirmed — depth amplified the 2008 shock |
| Green bar taller for `fdi_inflows` | Resilient countries had more external capital flowing in |
| Same pattern in both 2008 and 2020 charts | Financial development matters for resilience regardless of shock type |
| Different pattern between the two charts | The type of shock determines which financial characteristics protect you — the core finding |
