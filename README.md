# Walmart Sales Forecasting Report  

**Tools Used:** Excel · SQL (BigQuery) · Python (Pandas, Matplotlib, Seaborn, Statsmodels) · Tableau  

**Project Type:** Data Cleaning · Exploratory Data Analysis · Data Visualization  

**Relevant Link: [GitHub Repository](https://github.com/Edwardnam/Walmart-Sales-Forecasting)**
---

## Table of Contents  

- [Project Background](#project-background)  
- [Executive Summary](#executive-summary)  
- [Dataset Schema](#dataset-schema)  
- [Insights Deep-Dive](#insights-deep-dive)  
  - [Sales Trends and Growth Rates](#1-sales-trends-and-growth-rates)  
  - [Store Performance Leaderboard](#2-store-performance-leaderboard)  
  - [Holiday Impact](#3-holiday-impact)  
  - [Seasonality by Month and Year](#4-seasonality-by-month-and-year)  
  - [Correlation & Macroeconomic Drivers](#5-correlation--macroeconomic-drivers)  
  - [Additional Pivot Table Summaries](#6-additional-pivot-table-summaries)  
- [Dashboard (Tableau)](#dashboard-tableau)  
- [Recommendations](#recommendations)  
- [Clarifying Questions, Assumptions, and Caveats](#clarifying-questions-assumptions-and-caveats)  
- [About](#about)  

---

## Project Background  

Walmart, one of the largest U.S. retailers, wants to better understand drivers of weekly sales across its stores. This project analyzes **6,445 store-weeks (2010–2012)**, combining macroeconomic indicators such as fuel prices, CPI index, unemployment rates, and holiday flags.  

The objective is to:  
- Identify patterns in sales across time, stores, and external conditions.  
- Quantify the impact of holidays, seasonality, and macroeconomics.  
- Provide actionable insights to help management **improve forecasting and decision-making**.  

---

## Executive Summary  

- **Scale & Window:** 45 stores, 2010–2012, **6,445** weekly observations post-cleaning.  
- **Topline:** **$584M** total sales; **~$999K** average weekly sales per store.  
- **Growth:** 2011 up **~3–4%** vs 2010; 2012 down **~8–10%** vs 2011 (normalization after prior gains).  
- **Holidays:** Average **+7.8%** lift on holiday weeks; most pronounced in **Nov–Dec**.  
- **Store Inequality:** Top 10 stores contribute **35%+** of total sales → clear Tier 1/2/3 segments.  
- **Macro Sensitivity:** Fuel, unemployment, CPI show **weak** relationships with weekly sales (|r| < 0.12).  
- **So what:** Prioritize **holiday readiness** and **tier-based playbooks**; use **seasonality-aware forecasts** for inventory, labor, and promo planning.

### KPIs at a Glance
| Metric | Value |
|---|---|
| Total Sales | **$584M** |
| Stores | **45** |
| Rows (cleaned) | **6,445** |
| Avg Weekly / Store | **~$999K** |
| Holiday Lift | **+7.8%** |
| Top 10 Stores’ Share | **35%+** |

---

## Dataset Schema  

### Dataset Meta  

| Metric | Value |  
|---|---|  
| **Rows** | 6,445 (after cleaning) |  
| **Stores** | 45 |  
| **Date range** | 2010-02 to 2012-10 (weekly) |  
| **Target KPI** | `weekly_sales` (USD) |  

**Table Schema (SQL Extract)**  

| Column             | Data Type  | Description                               |  
|--------------------|------------|-------------------------------------------|  
| store              | INT64      | Store ID (1–45)                           |  
| date               | DATE       | Week start date                           |  
| weekly_sales       | FLOAT64    | Total weekly sales per store              |  
| holidays_flag      | STRING     | Holiday indicator (Holiday / No Holiday)  |  
| temperature        | FLOAT64    | Average weekly temperature (°F)           |  
| fuel_price         | FLOAT64    | Fuel price per gallon ($)                 |  
| cpi_index          | FLOAT64    | Consumer Price Index                      |  
| unemployment_rate  | FLOAT64    | Local unemployment rate (%)               |  
| year               | INT64      | Year extracted from date                  |  
| month              | INT64      | Month extracted from date                 |  

---

## Insights Deep-Dive  

### 1. Sales Trends and Growth Rates  

- Weekly trend shows **predictable seasonal spikes** centered on the U.S. holiday calendar; troughs cluster in **Feb/Jul**.  
- **12-week rolling averages** highlight sustained Q4 momentum and brief summer volatility.  
- 2010→2011 growth is broad-based; **2012 softness** is visible across most stores, consistent with post-recession normalization.  
- Volatility is **calendar-driven** rather than macro-driven (see Section 5).

📊 *Charts (Python)*:  
- ![Weekly Sales Over Time](assets/saleovertime.png)  
- ![Rolling Average Sales](assets/rolling_avg_sales.png)  

**Why this matters:** demand is **rhythmic and forecastable**. Inventory flow, staffing, and promo cadence should be **scheduled around the calendar**, not just recent sales.

---

### 2. Store Performance Leaderboard  

- **Top store (Store 10)** generated **$271M** total; lowest performers in the **$120–140M** range.  
- Clear **tiering** emerges:  
  - **Tier 1:** >$200M lifetime — high traffic, strong execution.  
  - **Tier 2:** $160–200M — average.  
  - **Tier 3:** <$150M — lagging.  
- Tier gaps persist across years → persistent structural/operational differences, not one-off shocks.

📊 *Chart (Tableau)*:  
- ![Store Leaderboard](assets/store_leaderboard.png)  

**Why this matters:** a **single playbook** under-serves both leaders and strugglers. Use **tier-specific targets** and interventions.

---

### 3. Holiday Impact  

- **Average lift = +7.8%** (t-test p < 0.05). Typical week **~$995K** → holiday week **~$1.07M**.  
- Lift is **durable across years**; peak sensitivity in **Nov–Dec** aligns with retail peak.  
- Holiday gains are **additive** to baseline seasonality (see Section 4), not merely noise.

📊 *Charts (Python + Tableau)*:  
- ![Holiday Impact Bar](assets/holiday_impact.png)  
- ![Weekly Sales Over Time (Tableau)](assets/weekly_sales_over_time_tableau.png)

**Why this matters:** holidays are **reliable profit pools**. Failing to front-load buys, labor, and promos leaves revenue and margin on the table.

---

### 4. Seasonality by Month and Year  

- Peak **December** average > **$1.25M**; troughs around **Feb/Jul** near **$950K**.  
- Heatmap and lines show **repeatable Q4 surges** with mild year-to-year variation.  
- Seasonality explains far more week-to-week variance than macro signals.

📊 *Charts (Python + Tableau)*:  
- ![Seasonality](assets/seasonality.png)  
- ![Seasonality by Month (Python)](assets/seasonality_by_month_python.png)

**Why this matters:** adding **month/week-of-year + holiday dummies** to forecasts should materially **reduce MAPE** and improve inventory placement.

---

### 5. Correlation & Macroeconomic Drivers  

- **Correlation Matrix Findings:** fuel (−0.01), unemployment (−0.11), CPI (−0.07) vs sales → **weak** relationships.  
- Scatter + LOWESS confirm **minimal slopes**; macro variables nudge but don’t **drive** weekly fluctuations.  
- Implication: focus on **internal levers** (assortment, promos, execution) over external volatility for weekly planning.

📊 *Charts (Python)*:  
- ![Correlation Matrix](assets/correlation_matrix.png)  
- ![Sales vs Unemployment (Python)](assets/sales_unem_python.png)  
- ![Sales vs Fuel (Python)](assets/sales_fuel_python.png)

---

### 6. Additional Pivot Table Summaries  

#### a) Sales by Year  

| Year | Total Sales | Avg Weekly Sales | Growth vs Prior Year |  
|------|-------------|------------------|----------------------|  
| 2010 | $200M+      | ~$980K           | – |  
| 2011 | $210M+      | ~$1.01M          | +3–4% |  
| 2012 | $175M+      | ~$975K           | –8–10% |  

➡ **Observation:** 2011 momentum slows in 2012 across most stores—consistent with macro normalization and fading stimulus effects.  

#### b) Average Weekly Sales by Store Tier  

| Store Tier | Definition            | Avg Weekly Sales | Key Insight |  
|------------|-----------------------|------------------|-------------|  
| Tier 1     | >$200M lifetime sales | $1.15M           | Flagship / high-traffic; protect availability |  
| Tier 2     | $160M–$200M           | ~$1.00M          | Solid base; improve conversion and attach |  
| Tier 3     | <$150M                | $0.85–0.90M      | Turnaround targets; fix leak points (OOS, price, floor set) |  

---

## Dashboard (Tableau)  

The Tableau dashboard consolidates KPIs and visuals:  

- **Filters:** Date Range, Holiday Flag  
- **KPI Cards:** Total Sales ($584M), Avg Weekly ($999K), Holiday Lift (+7.8%)  
- **Charts:**  
  - Weekly Sales Over Time (line)  
  - Store Performance Leaderboard (bar)  
  - Sales vs Unemployment (scatter + trend)  
  - Seasonality by Month-Year (heatmap)  
  - Sales vs Fuel Prices (scatter)  

📊 *Screenshot (Tableau)*:  
- ![Tableau Dashboard](assets/dashboard_tableau.png)

---

## Recommendations  

1. **Holiday Optimization (highest ROI)**  
   - Pull forward Q4 POs by **2–4 weeks**; target **>98%** availability on top SKUs.  
   - Phase promos (pre-BF warm-ups → Cyber week → last-mile gifting) to **smooth spikes** and protect margin.  
   - Expand **basket-builder bundles** and endcaps; pre-build displays before peak weeks.

2. **Tier-Based Store Playbooks**  
   - **Tier 1:** Maintain velocity; aggressive **attach/cross-sell** goals; protect service levels.  
   - **Tier 2:** Lift **conversion** via floor-set discipline, localized promos, and quick-win pricing tests.  
   - **Tier 3:** Diagnose **OOS %, price gaps, labor coverage**; pilot 2–3 turnaround kits and track lift.

3. **Forecasting Models**  
   - Baseline **Prophet/ARIMA** with **yearly seasonality + holiday dummies + store fixed effects**.  
   - Add **week-of-year**, **lag features**, and **promo flags**; retrain monthly; re-forecast weekly in Q4.  
   - Publish **MAPE by store** and use it to prioritize ops attention.

4. **Pricing & Promo Hygiene**  
   - Use **elasticity-aware** discounts; avoid blanket markdowns during strongest natural demand.  
   - Create **price fences** (bundles, membership, time-boxed offers) to capture value and protect margin.

5. **Measurement Plan**  
   - Track **Holiday OOS**, **Service Level**, **Attach Rate**, **Promo ROI**.  
   - Pre-register success criteria and (where possible) keep a **holdout** to measure true lift.

---

## Clarifying Questions, Assumptions, and Caveats  

- **Questions:**  
  - Can we split `holidays_flag` into specific holidays (e.g., Black Friday vs. Christmas) for more precise modeling?  
  - Can we enrich stores with **region/format/size** to explain tier gaps?  
  - Do we have **promo calendars** (type/depth/duration) to estimate price elasticity?

- **Assumptions & Caveats:**  
  - Data ends in **Oct 2012**; insights emphasize **in-sample patterns**.  
  - `holidays_flag` is binary; richer taxonomy would improve causal attribution.  
  - Macro variables are regional proxies; localized effects could be stronger than seen here.

---

## About  

This project demonstrates a **full-stack data workflow**:  
- **Excel** → Cleaning, pivot tables, preliminary metrics  
- **SQL (BigQuery)** → Aggregations, schema extraction, validation  
- **Python** → Time series, correlation, significance testing, visualizations  
- **Tableau** → Final dashboard for stakeholders  

For more projects and my data journey, connect on **[LinkedIn](https://www.linkedin.com/in/nam-ngo-b48780206/)**.  
