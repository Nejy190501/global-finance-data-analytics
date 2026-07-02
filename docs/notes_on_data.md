---
layout: default
title: Data Notes
nav_order: 3
description: "Overview of all six source files, their structure, join logic, and variable coverage."
---

# What is the data we work with?

The 6 files explained

  1. DataSheet_Main.csv — 20,216 rows × 68 cols

  What's in it: The main dataset. Each row = one country + one indicator, with yearly values as columns (1960–2023). It has ~76 different indicators per country covering banking, FDI, migration, remittances, etc.

  Example row:
  Country: Aruba | Indicator: International migrant stock (% of population)
  1990: 23.2%  |  1995: 27.7%  |  2000: 33.1%  |  2005: 32.5%  ...

  ---
  2. DataSheet_GDP.csv — 266 rows × 68 cols

  What's in it: Exactly one indicator — GDP growth (annual %) — for 266 countries, years 1961–2024. It was downloaded separately, probably to get more recent/complete GDP data.

  Example row:
  Country: Aruba | Indicator: GDP growth (annual %)
  2004: +7.3%  |  2009: -11.7%  |  2020: -23.9%  |  2021: +14.7%

  ---
  3. Metadata_Country.csv — 265 rows × 5 cols
  
  What's in it: One row per country with Region and Income Group labels. Used to tag countries so you can do group comparisons.

  Example row:
  Country Code: ABW | Country: Aruba | Region: Latin America & Caribbean | Income: High income

  ---
  4. Metadata_Country_GDP.csv — 265 rows × 5 cols
  
  What's in it: Identical structure to Metadata_Country.csv, just the companion file for the GDP dataset. Content is the same.

  ---
  5. Metadata_Indicators.csv — 76 rows × 4 cols

  What's in it: One row per indicator code with a full name, description, and source organization. Tells you what each code like SM.POP.TOTL.ZS actually means.

  ---
  6. Metadata_Indicator_GDP.csv — 1 row × 4 cols
  
  What's in it: Same as above but only for the one GDP indicator NY.GDP.MKTP.KD.ZG.

  ---
  How they join together

  DataSheet_Main.csv  ──┐
                        ├── joined on [Country Code] ──► Metadata_Country.csv
  DataSheet_GDP.csv   ──┘                                 (adds Region + IncomeGroup)

  DataSheet_Main.csv ── joined on [Indicator Code] ──► Metadata_Indicators.csv
                                                          (adds full description)

  The notebook then:
  1. Filters DataSheet_Main to only the 9 financial indicators of interest
  2. Pulls GDP growth out of DataSheet_GDP
  3. Concatenates them vertically (same shape)
  4. Melts from wide (years as columns) → long (one row per country-year)
  5. Pivots into a clean panel: one row = one country × one year, columns = indicators


# Final Dataset
    
1. Final dataset structure — what's joined how

  DataSheet_Main.csv       — 76 indicators × 266 countries × 64 years  (wide)
          +
  DataSheet_GDP.csv        — 1 indicator  × 266 countries × 64 years  (wide)
          ↓
  Concatenate vertically (same columns, GDP just adds rows)
          ↓
  Filter to 10 indicators of interest
          ↓
  Melt: years from columns → rows  (wide → long)
          ↓
  Pivot: indicators from rows → columns  (long → panel)
          ↓
  Join Metadata_Country on [Country Code]
          ↓
  Final panel: country + year + 10 indicator columns + Region + IncomeGroup

  Why GDP is a separate file: DataSheet_Main already has the GDP indicator code NY.GDP.MKTP.KD.ZG, but DataSheet_GDP.csv is a separate, more up-to-date download (updated 2026-04-08 vs 2024-03-28). So it gives you fresher/more complete GDP values.

  ---
  2. Income Group — is it one snapshot or time-series?
  
  It is a single snapshot — no year column. The World Bank assigns income groups based on GNI per capita thresholds updated every July, so in reality a country's income group can and does change over time (e.g. China moved from lower-middle to upper-middle to almost high-income over this dataset's 2004–2021 window).

  The metadata file only gives you the current classification (as of when it was downloaded). This is a real limitation worth mentioning in your analysis — you're applying today's label to 18 years of history. For your course project it's fine to use it as a structural grouping variable, but you'd note it's a simplification.

  ---
  3. Why melt the years into rows?

  Before melting (wide format):
  Country  | Indicator        | 2004 | 2005 | 2006 | ...
  Germany  | domestic_credit  | 95.1 | 97.3 | 99.2 | ...
  Germany  | gdp_growth       |  1.2 |  0.8 |  3.7 | ...

  After melt + pivot (panel format):
  Country  | Year | domestic_credit | gdp_growth | atms | ...
  Germany  | 2004 |           95.1  |        1.2 |  ... | ...
  Germany  | 2005 |           97.3  |        0.8 |  ... | ...
  Germany  | 2006 |           99.2  |        3.7 |  ... | ...

  One row = one country × one year. This is the standard panel data format that every statistical method (correlation, regression, groupby) works with naturally.

  ---
  4. All 76 indicators in DataSheet_Main
  
  The dataset covers 5 themes:

  ┌──────────────────────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
  │            Theme             │                                                Example indicators                                                 │
  ├──────────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
  │ Migration & remittances      │ Migrant stock, net migration, remittance cost/volume                                                              │
  ├──────────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
  │ Exchange rates & prices      │ Real effective exchange rate, CPI, WPI, inflation                                                                 │
  ├──────────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
  │ Financial access             │ Account ownership (%, by gender/age/education), bank branches, ATMs, borrowers/depositors                         │
  ├──────────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
  │ Financial depth & money      │ Domestic credit to private sector, broad money, monetary sector credit, interest rates (lending, deposit, spread) │
  ├──────────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤
  │ Capital markets & investment │ Stock market cap, stocks traded, FDI in/out, portfolio investment, nonperforming loans, reserves                  │
  └──────────────────────────────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘

  Your notebook picks 9 of the 76 plus GDP growth — all from the "financial depth" and "financial access" themes:

  NY.GDP.DEFL.KD.ZG.AD  →  inflation
  FS.AST.PRVT.GD.ZS     →  domestic_credit
  FR.INR.LNDP           →  interest_spread
  FM.LBL.BMNY.GD.ZS     →  broad_money
  FB.CBK.BRCH.P5        →  bank_branches
  FB.ATM.TOTL.P5        →  atms
  FB.AST.NPER.ZS        →  nonperforming_loans
  CM.MKT.LCAP.GD.ZS     →  market_cap
  BX.KLT.DINV.WD.GD.ZS  →  fdi_inflows
  NY.GDP.MKTP.KD.ZG     →  gdp_growth  (from DataSheet_GDP)