# Andrei — Presentation Script & Slide Templates
**Sections:** Introduction + SQ3 (2008) + SQ4 (COVID)
**Target time:** 8–9 minutes | 14 slides | ~40 seconds per slide

---

## INTRODUCTION

---

### SLIDE 1 — The Commission

**SLIDE TEXT**
> **"A Brief from the IMF"**
> - Two global shocks in 12 years — 2008 and 2020
> - IMF & World Bank need updated evidence on financial resilience
> - Our team: does financial depth protect economies when crises hit?

**SCRIPT**

Imagine you receive a brief from the IMF. The message is simple: two massive shocks in twelve years, and we still do not know whether countries with deeper financial systems actually hold up better when things go wrong. That is the question we are here to answer.

---

### SLIDE 2 — Why This Research Now?

**SLIDE TEXT**
> **"The Standard Advice Was Written Before Both Crises"**
> - Finance ministries — which reforms work for *their* income level?
> - IMF & World Bank — foundational research (King & Levine, 1993) predates both shocks
> - Investors & rating agencies — which indicators signal real resilience?
> - One-size-fits-all advice applied from Sub-Saharan Africa to Western Europe

**SCRIPT**

Why does this matter? Because the dominant advice — develop your financial sector, deepen credit markets — was established before 2008. Before COVID. The foundational studies, like King and Levine in 1993, were written in a world that had not yet seen either of these crises. And that same advice still gets applied identically to Sub-Saharan Africa and to Western Europe. Our analysis asks whether it actually holds up under stress.

---

### SLIDE 3 — The Data

**SLIDE TEXT**
> **"216 Countries. 18 Years. 5 Financial Indicators."**
> - World Bank WDI panel: 2004–2021
> - GDP growth + domestic credit, broad money, bank branches, ATMs, FDI
> - Indicators capture three dimensions: depth, access, and openness
> - One endogenous shock (finance) + one exogenous shock (public health) — same dataset

**SCRIPT**

Our data is the World Bank's World Development Indicators. 216 countries, 18 years. For every country, every year — GDP growth and five measures of financial development. We chose these five deliberately: domestic credit and broad money capture depth, bank branches and ATMs capture access, and FDI captures openness to global capital. Crucially, the window contains two structurally opposite crises — one that started inside the financial system, one that came from completely outside it. That contrast is what makes this dataset unusually powerful.

---

### SLIDE 4 — The Central Question

**SLIDE TEXT**
> **"The Central Question"**
> *Does financial depth protect an economy when a crisis hits — or does it expose it?*
> And does the answer change depending on where the shock comes from?

**SCRIPT**

This is the question we set out to answer. The full version: which dimensions of financial sector development explain differences in GDP growth across income groups — and how did those relationships hold up through 2008 and 2020? But the core of it is simpler: is financial depth a shield, or an exposure? Take a moment with it, because everything that follows is built to answer it.

---

### SLIDE 5 — How We Answer It

**SLIDE TEXT**
> **"Five Angles — Two Are Mine"**
> - The Financial Landscape — who are the players and how do they differ?
> - What Drives Growth in Normal Times — which channels matter most?
> - **→ The 2008 Crisis — did financial depth protect or amplify?** *(Andrei)*
> - **→ The COVID Shock — same story or different crisis entirely?** *(Andrei)*
> - How Banking Access Is Shifting — branches to ATMs and what it means

**SCRIPT**

To answer it properly we broke it into five angles. My colleagues cover the financial landscape, the normal-times drivers, and the banking access shift. I take you through the two crises. That is where the story gets surprising — because 2008 and 2020 do not tell the same story at all.

---

## SQ3 — THE 2008 GLOBAL FINANCIAL CRISIS

---

### SLIDE 6 — What Was 2008?

**SLIDE SUBTITLE**
> *The 2008 crisis was not bad luck. It was built into the system designed to drive growth — which is exactly why the most financially developed countries fell the hardest.*

**SLIDE FORMAT: TIMELINE — 5 columns with year + short text below each**

| **2004** | **2007** | **Mar 2008** | **Sep 2008** | **2009** |
|---|---|---|---|---|
| US housing boom. Banks bundle subprime mortgages into mortgage-backed securities (MBS) — sold to investors worldwide. | Housing prices fall. MBS values collapse. First major bank losses emerge. The cracks are visible. | Bear Stearns fails. JP Morgan and the Fed orchestrate a rescue — the first sign the system is breaking. | Lehman Brothers files for bankruptcy. Global credit freezes overnight. The shock spreads across every connected market. | Global recession. High-income economies contract sharply. The most financially integrated countries fall the hardest. |

**Image suggestion for top half:** photo of Lehman Brothers headquarters / traders on the floor / newspaper front page from Sept 2008

**SCRIPT**

Before we look at the data, a quick anchor. 2008 was not a war, a pandemic, or a natural disaster. It started inside American mortgage markets — banks had bundled risky home loans into mortgage-backed securities and distributed them across the global financial system. When US housing prices fell, those securities became worthless. Bear Stearns was first — rescued in March. Then Lehman Brothers filed for bankruptcy in September, and global credit froze overnight. What made 2008 structurally unusual — and what matters for everything we show next — is that the shock originated inside the financial system itself.

---

### SLIDE 7 — The Trajectory

**SLIDE TEXT**
> **"2008 Hit Everyone — But Not Equally"**
> - High-income countries: ~+3% growth in 2007 → ~−3.5% in 2009
> - Low-income countries dropped far less — less integrated into global finance
> - The most financially developed economies fell the hardest
> - **If depth protected, you would expect the opposite**

**SCRIPT**

*[point to line chart]*

GDP growth by income group, 2004 to 2014, crisis years shaded. High-income countries went from around positive three percent in 2007 to negative three-and-a-half in 2009. Low-income countries dropped far less — not because they were stronger, but because they were less connected to global capital markets. If financial depth were protective, you would expect the most developed economies to hold up best. They did not.

---

### SLIDE 8 — Who Got Hit and Why

**SLIDE TEXT**
> **"Financially Integrated Countries Fell the Most"**
>
> **Hardest hit — high-income, deeply integrated:**
> - Latvia −27pp, Estonia −23pp, Lithuania −23pp below pre-crisis trend
> - USA: +2.2% (2007) → −2.6% (2009)
> - Germany: +3.3% (2007) → −5.6% (2009)
>
> **Most resilient — low-income, minimally connected:**
> - Tanzania: stayed at +6% throughout — barely moved
> - Ethiopia: +10% in 2009 — the crisis simply did not reach them
>
> **Why:** deep credit markets and large FDI = direct exposure to global contagion. No integration = no transmission.

**CODE SNIPPET (show on slide to explain the deviation score)**

```python
# 1. Baseline: each country's avg growth in the calm years 2004–2007
pre_crisis_avg = df[df.Year.between(2004, 2007)] \
                   .groupby("Country")["gdp_growth"].mean()

# 2. Crisis year value: growth in 2009
gdp_2009 = df[df.Year == 2009].set_index("Country")["gdp_growth"]

# 3. Deviation score = how far the country fell below its OWN trend
deviation = gdp_2009 - pre_crisis_avg
```

> **In words:** `deviation = growth(2009) − average growth(2004–2007)`
> Negative = fell below its own normal pace. More negative = harder hit.

**SCRIPT**

First, how we measured the hit. For every country we took its average GDP growth in the four calm years before the crisis — 2004 to 2007 — as its own baseline. Then we subtracted that baseline from its growth in 2009, the worst crisis year. So the number on each bar is not raw GDP — it is how far a country fell below its *own* normal pace. This matters: a country that usually grows at ten percent and drops to minus fourteen fell twenty-four points below itself, even though another country sitting at zero looks worse on paper. We are measuring the size of the shock relative to each country's own trend.

On that measure, the hardest hit — Latvia, Estonia, Lithuania, but also the US and Germany — were all deeply integrated into global financial markets. Latvia fell almost twenty-seven points below its own baseline. Now compare that to Tanzania, Ethiopia, and Uganda — all within a few points of their normal growth. Not because they had stronger institutions. Because they were barely connected to the global financial system that was collapsing. The shock had no channel to travel through. Financial depth was not a shield in 2008. It was a transmission channel.

---

### SLIDE 9 — The 2008 Finding

**SLIDE TEXT**
> **"The Channel That Drives Growth Also Spreads the Crisis"**
> - Positive correlation: higher pre-crisis financial integration → larger growth drop
> - The same FDI and credit flows that fuel normal-times growth transmitted the shock
> - When the shock comes from inside finance — depth means exposure
> - One-size-fits-all resilience advice does not survive 2008

**SCRIPT**

The standard assumption — more finance equals more resilience — does not survive 2008. We find a positive relationship between a country's pre-crisis financial depth and how hard it fell: the deeper the integration, the greater the damage. The channel that drives growth in normal times — credit, FDI, financial openness — became the channel through which the crisis spread. Now — does COVID tell the same story?

---

## SQ4 — THE COVID-19 SHOCK 2020

---

### SLIDE 10 — What Was COVID?

**SLIDE SUBTITLE**
> *COVID was not born inside finance. The shock came from outside the economy entirely — which is exactly why a completely different kind of country got hit.*

**SLIDE FORMAT: TIMELINE — 5 columns with date + short text below each (mirror the 2008 slide)**

| **Dec 2019** | **Mar 2020** | **Apr 2020** | **2020** | **2021** |
|---|---|---|---|---|
| Virus emerges in Wuhan, China. Still a regional health story. | WHO declares a global pandemic. Borders close worldwide within weeks. | Half the planet under lockdown. Travel, tourism, and trade stop almost overnight. | Global GDP contracts ~3.1% — the worst peacetime recession since the 1930s. | Recovery begins — but deeply uneven across countries. |

**Image suggestion for top half:** empty airport / closed border / deserted tourist destination

**SCRIPT**

Same structure, completely different crisis. COVID did not start in a bank. It started with a virus in Wuhan in late 2019. By March 2020 the WHO declared a pandemic and borders slammed shut across the globe. Within weeks, half the planet was in lockdown — and travel, tourism, and trade simply stopped. The result was a three-point-one percent global contraction, the worst since the Great Depression. But notice the key difference from 2008: this shock came from completely outside the financial system. That single fact changes who gets hurt.

---

### SLIDE 11 — The Trajectory

**SLIDE TEXT**
> **"COVID Hit Differently — Tourism, Not Finance"**
> - All income groups collapsed in 2020 — high and low income alike
> - But this time the most financially developed were *not* the hardest hit
> - Hardest hit: tourism-dependent economies, regardless of financial depth
> - Low-income countries dropped less — less exposed to international travel

**SCRIPT**

*[point to line chart]*

Same chart as before, now 2017 to 2021. Everyone collapsed in 2020. But look at the pattern compared to 2008: the income groups fell much closer together, and the most financially developed economies are no longer clearly the worst hit. Financial depth — which was the dividing line in 2008 — barely separates anyone here. Something other than finance is driving who falls hardest.

---

### SLIDE 12 — Who Got Hit and Why

**SLIDE TEXT**
> **"This Time, Tourism — Not Finance — Decided Who Fell"**
>
> **Hardest hit — tourism-dependent economies:**
> - Turks & Caicos, Macao, Maldives, Aruba — all collapsed 25–60pp below trend
> - Their financial systems were *shallow or average* — not deep like 2008's victims
>
> **Most resilient:**
> - Diversified and non-tourism economies barely moved
>
> **Why:** the shock hit physical movement, not credit. Financial depth had nothing to transmit — and nothing to protect.

**SCRIPT**

*[point to bar chart]*

Same method as 2008 — how far each country fell below its own pre-crisis trend. But look who is at the top now. The Maldives, Macao, Aruba, Turks and Caicos — economies built almost entirely on international tourism. When borders closed, their income vanished overnight. And here is the critical point: these countries were *not* financially deep. The thing that predicted disaster in 2008 — heavy credit and FDI — is simply absent here. Financial structure did not protect them, and it did not expose them. It was irrelevant. What mattered was whether your economy depends on people physically crossing borders.

---

### SLIDE 13 — Different Victims

**SLIDE TEXT**
> **"2008 and 2020 Hit Different Countries"**
>
> **2008 worst hit — financially integrated:**
> Latvia, Estonia, Lithuania, Iceland, Ukraine, Russia, Armenia
>
> **2020 worst hit — tourism-dependent:**
> Macao, Maldives, Bahamas, Fiji, Seychelles, St. Lucia, Cabo Verde
>
> **Only 5 of 20 appear in both lists** — Macao, Maldives, Aruba, Antigua, Monaco (all tourism/small-state)
>
> - **The financially-integrated core of 2008 is absent from 2020**
> - The origin of the shock determines the victim

**SCRIPT**

*[point to comparison chart]*

So were the same countries hit hard in both crises? Almost none. We took the twenty hardest-hit countries from each crisis and compared the lists — only five appear in both, and tellingly, those five are tourism-dependent small states like the Maldives and Macao, which were exposed to both shocks for different reasons. But the financially-integrated core of 2008 — Latvia, Estonia, Lithuania, Iceland, Ukraine, Russia — does not appear in the 2020 list at all. The countries that deep finance made vulnerable in 2008 were simply not the countries that tourism dependence made vulnerable in 2020. That is the finding: the origin of the shock determines the victim.

---

### SLIDE 14 — The Punchline

**SLIDE TEXT**
> **"Financial Structure Predicts 2008 — But Not 2020"**
> - Model trained on 2008 financial profiles: **R² = +0.16** — weak but real signal
> - Same model applied unchanged to 2020: **R² = −0.16** — not just weak, actively wrong
> - The 2008 playbook predicts the *inverse* of what happened in 2020
> - **Resilience strategy must match the type of crisis expected**

**HOW THE MODEL WORKS (show on slide)**

- **Model:** ordinary linear regression (`sklearn.LinearRegression`)
- **Inputs (X):** 5 pre-crisis financial indicators per country — domestic credit, broad money, bank branches, ATMs, FDI
- **Target (y):** the deviation score — how far each country fell below its own trend
- **Train:** fit on 2008 (2004–2007 profiles → 2009 drops)
- **Test:** apply the *same, unchanged* model to 2020 (2017–2019 profiles → 2020 drops)
- Only countries with complete data in *both* crises are used

```python
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score

FEATURES = ["domestic_credit", "broad_money",
            "bank_branches", "atms", "fdi_inflows"]

# train on 2008: financial profile -> how hard each country fell
model = LinearRegression().fit(X_2008[FEATURES], drop_2008)

# 1) how well does it fit 2008?      2) does it transfer to 2020?
r2_2008 = r2_score(drop_2008, model.predict(X_2008[FEATURES]))   # = +0.16
r2_2020 = r2_score(drop_2020, model.predict(X_2020[FEATURES]))   # = -0.16
```

**SCRIPT**

To test this rigorously we built a simple linear regression — five inputs, the pre-crisis financial profile of each country: domestic credit, broad money, bank branches, ATMs, and FDI. The target it learns to predict is the deviation score from earlier — how hard a country fell below its own trend. We train it only on 2008: feed it each country's 2004-to-2007 financial profile, and let it learn the relationship to the 2009 drop. Then — and this is the key step — we take that exact same model, unchanged, and hand it the pre-2020 profiles to predict the COVID drops.

On 2008 it captures some of the variation — R-squared of positive 0.16. Weak, but the signal is real. Applied to 2020, R-squared falls to negative 0.16. The model does not just fail — it predicts the inverse of what happened. Countries it flagged as vulnerable held up fine; countries it called safe collapsed.

That is not a modelling problem. That is a structural finding.

The answer to our central question: financial depth does not universally protect or expose. It depends entirely on where the shock originates. When the crisis comes from inside the financial system, depth transmits it. When it comes from outside, financial structure is largely irrelevant. There is no single resilience strategy. The type of crisis you expect must shape the defences you build.

---

*Total estimated time: 8–9 minutes*
