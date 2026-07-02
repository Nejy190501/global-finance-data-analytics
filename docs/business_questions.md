---
layout: default
title: Business Questions
nav_order: 1
description: "Main research question, sub-questions, stakeholder motivation, and scope."
---

# Financial Development and Economic Growth — Research Brief

**Team 2:** Andrei Piatrouski, Nikita Pasichnyk, Hannes Gantzkow, Jean Yves Nkwane Ebongue
**Course:** Introduction to Data Analytics, HWR Berlin, SoSe 2026
**Dataset:** World Bank World Development Indicators — 216 countries, 2004–2021, 10 financial and macroeconomic indicators

---

## Project framing

Picture the following commission. The IMF Research Department, working with the World Bank's Finance for Development team, asks a small analyst group (us) to revisit the "finance-growth nexus" using the 2004–2021 cross-country panel of financial indicators. The timing is deliberate: the world has now lived through two global shocks within fifteen years — the 2008 global financial crisis and the 2020 COVID-19 pandemic — and the institutions want refreshed, data-driven guidance on whether deeper financial systems still pay off for growth, **for whom** the payoff is largest, and **how well it holds up** when crises hit.

The deliverable is an evidence brief that an IMF country mission or a World Bank sector team could pick up before drafting policy advice on financial-sector reform in a low-, lower-middle-, upper-middle- or high-income economy.

---

## Stakeholders and what they need from this analysis

**Finance ministries and central banks** need to know which financial-sector reforms actually move the growth needle for an economy at their income level. Advice that works for a high-income economy rarely translates to a low-income one, yet generic prescriptions are common. They want the channel-by-income-group breakdown, not a single global average.

**Development economists at the IMF and World Bank** need updated empirical evidence to refresh their advisory frameworks. Foundational finance-growth research (King and Levine 1993; Levine 2005) predates both the 2008 crisis and the rapid expansion of digital and mobile finance. A 2004–2021 look — covering two major shocks and a structural shift in financial access — directly informs surveillance reports and program design.

**International financial institutions (IMF, World Bank, regional development banks)** tie lending and technical assistance to financial-sector benchmarks (credit-to-GDP, ATMs per 100k adults, branches per 100k adults, etc.). They want to know whether these benchmarks actually track growth-relevant variation, and whether the implicit thresholds need to be recalibrated by income tier.

**Investors, rating agencies, and DFI risk teams** use financial-development metrics inside country risk models. Knowing which financial-development dimensions held up best across the two recent shocks helps sharpen sovereign resilience indicators.

---

## Why a new look is worth doing now

Three reasons make the 2004–2021 window analytically valuable:

1. It contains **two structurally different global shocks** — one originating inside the financial system (2008) and one external to it (2020). Comparing them is a natural way to test whether financial development matters for resilience in general or only for finance-driven crises.
2. The period spans the most rapid **shift in retail banking access** in modern history, with ATMs and digital channels growing while branch networks plateaued or shrank in many countries. The growth implications of this shift are still actively debated.
3. The post-2008 literature opened the question of whether **some forms of financial depth can become destabilizing**. Pre-crisis literature treated finance as unambiguously growth-enhancing; the post-crisis view is more nuanced. Our window straddles both regimes.

---

## What the data lets us answer

Our cleaned panel has 216 countries × 18 years × 10 indicators, with region and income-group metadata. The variables with the strongest coverage — GDP growth, inflation, FDI inflows, bank branches, ATMs, domestic credit, broad money — are precisely the variables at the center of the finance-growth debate. That alignment is what makes the project tractable.

Variables with heavy missingness (market capitalization ~65% missing, nonperforming loans ~52%, interest-rate spreads ~43%) will play a supporting role rather than drive the main conclusions. Income group is a single World Bank snapshot, so we treat it as a structural grouping rather than a time-varying truth.

---

## Main research question

> **Which dimensions of financial sector development most strongly explain differences in GDP growth across income groups between 2004 and 2021, and how did these relationships evolve through the 2008 global financial crisis and the 2020 COVID-19 shock?**

This question is built to be explanatory and multi-dimensional rather than a yes/no test. A yes/no answer would not satisfy it. To answer it we have to:

- identify **multiple** financial-development channels (not treat finance as one variable),
- rank channels **separately by income group** (not produce a single global coefficient),
- track how the rankings **shift across the two crisis windows** (not report one static estimate).

---

## Sub-questions

The five sub-questions decompose the main question into a sequence: describe the landscape, identify the strongest relationships, stress-test them through two crises, and trace a structural shift in financial access. Each is phrased to require an explanatory answer, not a yes/no.

### SQ1 — Mapping the financial landscape

> **How do countries differ in their financial-development profile — credit depth, monetary depth, banking access, and external capital — and to what extent do these profiles align with, or diverge from, the World Bank's income classification across the 2004–2021 window?**

**Why it belongs to the main question.** Before we can rank channels by income group, we need to verify that the income label is actually a meaningful financial-system descriptor. Income groups are a convenient shorthand, but countries inside the same label can have very different financial structures. SQ1 supplies the typology the later sub-questions refer back to.

**How we answer it with the data.** Groupby-level descriptive statistics by income group and region across the seven best-covered indicators; distributional comparison (median, IQR) over the full window; an exploratory cluster analysis on a country-level average profile to see whether the data-driven groups echo or contradict the income label.

### SQ2 — Ranking the channels in normal times

> **Which channels of financial development — credit depth (domestic credit), monetary depth (broad money), banking access (bank branches and ATMs), and external capital (FDI inflows) — show the strongest association with GDP growth in the non-crisis years, and how does the ranking change between low-, lower-middle-, upper-middle- and high-income groups?**

**Why it belongs to the main question.** This is the structural-relationship core. The main question asks which dimensions matter; SQ2 answers it for the baseline (non-crisis) periods, providing the benchmark that the crisis sub-questions will compare against.

**How we answer it with the data.** Pearson and Spearman correlations between each channel and contemporaneous GDP growth, repeated inside each income group; a small multivariate OLS with inflation as a control to isolate each channel's marginal association; channel ranking tabulated per income tier.

### SQ3 — Behaviour during the 2008 global financial crisis

> **In what ways did the relationship between financial-sector development and GDP growth shift during and after the 2008 global financial crisis, and which financial-development characteristics distinguished the countries that contracted least from those hit hardest?**

**Why it belongs to the main question.** The 2008 crisis is the natural experiment for asking whether deeper financial systems amplify or absorb shocks. The post-crisis literature opened the possibility that some forms of financial depth are destabilizing — SQ3 tests that on our data.

**How we answer it with the data.** Window split (2004–2007 pre-crisis, 2008–2009 crisis, 2010–2014 recovery); per-country growth deviation from each country's own pre-crisis trend; comparison of financial profiles between the best- and worst-performing deciles during the crisis years, by income group.

### SQ4 — Behaviour during the 2020 COVID-19 shock and comparative recovery

> **How did the 2020 COVID-19 shock affect GDP growth across countries with different financial-development levels, and in which respects does the 2020–2021 recovery pattern resemble — or diverge from — the 2009–2010 recovery from the global financial crisis?**

**Why it belongs to the main question.** One shock could be idiosyncratic; two structurally different shocks let us tell whether financial development matters for resilience in general or only for crises that originate inside the financial system. SQ4 turns the analysis into a comparative natural experiment.

**How we answer it with the data.** The same window-based methodology as SQ3 applied to 2018–2021; side-by-side tabulation of the financial-development characteristics most associated with faster recovery in each crisis; explicit comparison of which channels look stabilising in one episode and not the other.

### SQ5 — The shift in retail financial access

> **How has the balance between physical bank branches and ATM networks evolved across income groups between 2004 and 2021, and to what degree does the speed of this shift track GDP growth performance in lower- and lower-middle-income economies?**

**Why it belongs to the main question.** SQ2–SQ4 treat finance largely as a stock (depth, access at a point in time). SQ5 adds the structural-change angle: financial access itself is being reshaped during our window, and the policy debate today is precisely about whether that reshaping is helping or just substituting. This is the forward-looking sub-question.

**How we answer it with the data.** Time series of branches per 100k adults and ATMs per 100k adults, broken down by income group and region; identification of countries with the fastest substitution from branches to ATMs; pairing the substitution speed against GDP growth within lower-income economies.

---

## How the sub-questions build the answer to the main question

- **SQ1** gives us the map. We learn who the players are and whether the income label is a real grouping.
- **SQ2** identifies the strongest finance-growth links in normal times — the baseline.
- **SQ3** and **SQ4** stress-test that baseline through two structurally different shocks, with each crisis acting as a robustness check on the other.
- **SQ5** captures a structural transformation of financial access running through the entire window, which neither the static profile (SQ1) nor the channel ranking (SQ2) can show on its own.

Reading the five answers together gives the main question a layered answer: **which channels matter, for whom, under which conditions, and where the system is heading.** A single yes/no answer cannot deliver that; a sequence of explanatory answers can.

---

## Honest scope and limits

- The analysis can establish strong **associations and patterns**, but with this data alone it cannot prove causation in the econometric sense (no instruments, no identification strategy beyond the two shocks).
- The income-group label is a **single World Bank snapshot**; the historical classification of several countries varies during the window. We apply today's label across 18 years of data and treat it as a structural category, not a time-varying truth.
- Several indicators (market cap, nonperforming loans, interest-rate spreads) have over 40% missingness, particularly in lower-income countries; we will not lean on these as primary explanatory variables.
- Mobile and digital finance — arguably the most consequential financial-inclusion development of our window — is not captured by this dataset and remains an explicit blind spot we will acknowledge.
- The 2022–2024 period is largely missing for most indicators in the main file, so the inflation aftermath of COVID lies outside our window.

These limits are not weaknesses of the question; they are the boundary that keeps the conclusions honest and the brief usable by a policy reader who would otherwise have to second-guess them.

---

## Note: why post-2021 events are out of scope

A reader looking at the world in mid-2026 would reasonably ask whether the current geopolitical restructuring — the Iran conflict, the broader Middle East escalation, the energy-price reshuffling, the ongoing reconfiguration of supply chains and reserve-currency politics — belongs inside a brief on financial development and growth. The short answer is that it does not, and for reasons that are about the data, not about relevance.

**The data window ends in 2021.** The World Bank panel we work with has effectively no coverage for the variables we depend on (domestic credit, broad money, branches, ATMs, FDI) for 2022 onward, and only partial coverage even for GDP growth and inflation. Adding a sub-question about a 2025–2026 shock with no observations on either side of it would not be analysis — it would be commentary dressed up as analysis.

**Two crises are enough to make the methodological point.** The brief uses 2008 and 2020 because they sit cleanly inside our window with several years of pre- and post-shock data on each side. That structure is what lets us compare resilience profiles. A third shock without symmetric pre/post coverage would not add evidence; it would add noise.

**Even with hypothetical updated data, a 2024–2026 shock is too recent for our methodology.** The window-based comparison in SQ3 and SQ4 needs at least two to three post-shock years of recovery data to characterise a recovery trajectory. As of May 2026, the post-event period is still unfolding.

**The brief is still forward-relevant.** The channels we identify, the income-group differences we map, and the resilience patterns we draw from 2008 and 2020 are exactly the inputs a policy team would use to think about the financial-sector exposure of countries entering the current period. We are providing the structural framework; the IMF country desks will overlay today's specifics on top of it.

This is a natural follow-up project once the World Bank publishes the 2022–2024 updates, and a deliberate one for any team revisiting the brief with 2025–2027 data once it becomes available.
