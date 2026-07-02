# Nikita — Presentation Script & Slide Templates
**Section:** SQ1 — The Financial Landscape
**Target time:** ~6 minutes | 6 slides | ~60 seconds per slide

---

## SQ1 — THE FINANCIAL LANDSCAPE

---

### SLIDE 1 — The Question

**SLIDE TEXT**
> **"Does the Income Label Describe a Country's Financial System?"**
> *At the extremes, yes. In the middle, only roughly.*
> - 186 countries · 5 financial indicators · 2004–2021

**SCRIPT**

My part is the map — the financial landscape. Before we rank financial channels by income group in the later sections, one assumption has to hold: that a country's income label actually describes its financial system. So I tested it. The short answer, up front: at the extremes it holds — at the very bottom and the very top. In the middle, only roughly. Let me show you how the data gets there.

---

### SLIDE 2 — The Profiles

**SLIDE TEXT**
> **"Banking Depth Climbs with Income — FDI Is the Exception"**
> - Every banking indicator rises step-by-step from low to high income
> - High-income credit depth ≈ 8× the low-income level (82% vs 11% of GDP)
> - FDI is flat across all groups — external capital follows opportunity, not income

**FIGURE:** median-profile heatmap — income group × 5 indicators (domestic credit, broad money, bank branches, ATMs, FDI)

**SCRIPT**

*[point to heatmap]*

This is the starting picture: for each income group, the median of five financial indicators. Read it top to bottom, poor to rich. The four banking measures — credit, broad money, branches, ATMs — all climb together; high-income credit depth is about eight times the low-income level. One column breaks the pattern: FDI is flat across every group, because external capital chases opportunity and tax treatment, not income — and that is exactly what produces the outliers I will show in a minute. One honest note on the chart: the colour is scaled within each column, so even flat FDI looks like a gradient — read the numbers, not just the shade. These are the five best-covered indicators; we left out market cap, which is two-thirds missing.

---

### SLIDE 3 — The Test

**SLIDE TEXT**
> **"Hide the Labels: the Data Finds a Ladder, Not Four Tiers"**
> - Method: standardize the 5 indicators, then k-means with k = 4 — set to match the 4 income groups
> - Result: not four income tiers, but a smooth ladder of financial depth (mean credit 21 → 63 → 140 → 143% of GDP)
> - Cluster 4 is just two countries — Cyprus & Malta

**HOW THE CLUSTERING WORKS** *(optional — show on slide)*

```python
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans

FEATURES = ["domestic_credit", "broad_money",
            "bank_branches", "atms", "fdi_inflows"]

# one row per country = its 2004–2021 average, then standardized
X = StandardScaler().fit_transform(profile[FEATURES])

# k = 4, to test directly against the 4 World Bank income groups
clusters = KMeans(n_clusters=4, random_state=0, n_init=10).fit_predict(X)
```

**SCRIPT**

*[point to the ladder]*

Now the real test. I hide the income labels and let k-means group countries on their financial fingerprint alone. Two deliberate choices I should flag: I standardize the indicators first — credit runs over a hundred while FDI is single-digit, so without it the big numbers would dominate — and I set k to four, on purpose, to test against the four income groups. The result: the algorithm does not rebuild the income tiers. It finds a ladder of financial depth — mean credit climbing twenty-one, sixty-three, a hundred-forty, a hundred-forty-three. One honest point: that top cluster is just two countries — a two-country cluster is fragile by definition, but that is the finding, because they are genuinely extreme. Which two? That is the next slide.

---

### SLIDE 4 — Data vs Labels

**SLIDE TEXT**
> **"If Income Equalled Financial Depth, This Would Be a Clean Diagonal"**
> - The drift is real: poorer = shallow, richer = deep
> - But it is smeared, not diagonal — upper-middle income splits across three clusters (22 shallow / 26 medium / 2 deep)
> - Even high-income isn't one group — it spreads from shallow to extreme

**FIGURE:** cluster × income cross-tab heatmap (rows = data clusters, columns = income labels, n = 186)

**SCRIPT**

*[point to the cross-tab]*

This is the evidence behind "a ladder, not tiers." I cross the four data clusters — the rows — against the four income labels — the columns. If income perfectly described financial depth, every country would sit on a clean diagonal. What we actually see: the drift is real — poorer countries pile into the shallow cluster, richer ones into the deeper ones — so income is not meaningless. But it is smeared, not diagonal. Upper-middle income is the worst case: it splits across three clusters at once — twenty-two shallow, twenty-six medium, two deep. And here is the most important pattern, right there in the zeros: the deep clusters hold no low-income country — none — but the shallow cluster does hold two high-income ones. The errors run one way only. Financial depth can lag behind income, but it never runs ahead of it. Wealth is necessary for a deep financial system — but not sufficient.

---

### SLIDE 5 — The Outliers

**SLIDE TEXT**
> **"Four Countries Show What the Income Label Misses"**
> - **Cyprus & Malta** — small, rich financial hubs; FDI near 90–100% of GDP; their own micro-cluster
> - **Guyana & Trinidad and Tobago** — high-income from oil & gas, but among the shallowest systems
> - The asymmetry: no low-income country looks financially rich — depth can lag income, never lead it

**FIGURE:** FDI vs domestic credit scatter (country averages, coloured by income group)

**SCRIPT**

*[point to the scatter]*

These are the countries where the label and the reality come apart. Far right: Cyprus and Malta — small, rich economies whose financial sector dwarfs GDP, with FDI near ninety to a hundred percent. That is the FDI exception made visible, and why they get their own cluster. Lower-left, despite being high-income: Guyana and Trinidad and Tobago — rich from oil and gas, but among the shallowest financial systems in the data. Resource wealth lifts income without building deep finance. And notice the bigger pattern: the blue high-income dots are scattered across the whole credit range, so "high income" is not one financial block — and no low-income country ever climbs into the top. The asymmetry again, in raw form.

---

### SLIDE 6 — The Takeaway

**SLIDE TEXT**
> **"Income Is a Good Proxy for Finance — at the Edges, Rough in the Middle"**
>
> **For our analysis:** grouping SQ2–SQ5 by income is legitimate — but read middle-income results with care, and keep the outliers in view.
>
> **For the policy reader:** income-tier benchmarks (credit/GDP, ATMs per 100k) fit best at the extremes. For a middle-income economy, don't infer financial structure from the income label alone.
>
> *Scope: 186 of ~215 countries clustered; those dropped skew poorer.*

**SCRIPT**

So, the verdict in one line: income is a good proxy for financial development at the extremes, and only a rough one in the middle. For our project, that is a green light with a caveat — grouping the later questions by income is fair, but middle-income results need care. And a subtler warning: the banking indicators all move together, so no single one can be cleanly credited for growth — which is the direct handover to the channel ranking next. The honest boundary, before anyone asks: only a hundred-eighty-six of about two-hundred-fifteen countries had complete-enough data, and the ones we dropped skew poorer — so my weakest evidence is exactly the low-income group. With the map drawn, the channel ranking comes next.

---

*Total estimated time: ~6 minutes*
