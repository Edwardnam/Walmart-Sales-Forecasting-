# Walmart Sales Forecasting Report  

**Tools Used:** Excel · SQL (BigQuery) · Python (Pandas, Matplotlib, Seaborn, Statsmodels) · Tableau  

**Project Type:** Data Cleaning · Exploratory Data Analysis · Data Visualization  

**Relevant Link:** [GitHub Repository](#)  

---

## Table of Contents  

1. Project Background  
2. Executive Summary  
3. Dataset Schema  
4. Insights Deep-Dive  
   - Sales Trends and Growth Rates  
   - Store Performance Leaderboard  
   - Holiday Impact  
   - Seasonality by Month and Year  
   - Correlation & Macroeconomic Drivers  
5. Dashboard (Tableau)  
6. Recommendations  
7. Clarifying Questions, Assumptions, and Caveats  

---

## Project Background  

Walmart, one of the largest U.S. retailers, wants to better understand drivers of weekly sales across stores. This project combines **6,445 weekly records (2010–2012)** with macroeconomic indicators like fuel prices, CPI index, unemployment rates, and holiday flags to evaluate patterns, forecastability, and actionable levers for sales.  

---

## Executive Summary  

- Walmart’s sales dataset covers **45 stores, 2010–2012**, totaling **$584M in sales**.  
- Average weekly sales hover around **$999K per store**, with **+7.8% holiday lift**.  
- **Top 10 stores** generate disproportionately high sales ($180M–$270M each).  
- Seasonality: Sales peak in **November/December** (holiday season), trough in summer.  
- Macroeconomic variables (fuel price, unemployment, CPI) show **weak but negative correlations** with weekly sales.  
- Forecast dashboards and rolling averages indicate **short-term predictability**, but strong outliers (holiday spikes) make raw trends volatile.  

---

## Dataset Schema  

### Dataset Meta  

| Metric | Value |  
|---|---|  
| **Rows** | 6,445 (after cleaning) |  
| **Stores** | 45 |  
| **Date range** | 2010-02 to 2012-10 (weekly) |  
| **Geography** | United States (store-level) |  
| **Target KPI** | `weekly_sales` (USD) |  

**Table Schema** (SQL Extract)  

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

- Weekly sales show **strong spikes during holidays** (Nov/Dec).  
- **12-week rolling averages** smooth out volatility, showing consistent end-of-year peaks.  

📊 *Charts (Python)*:  
- ![Weekly Sales Over Time](assets/charts/weekly_sales.png)  
- ![Rolling Average Sales](assets/charts/rolling_avg_sales.png)  

---

### 2. Store Performance Leaderboard  

- Top stores significantly outperform others (Store 10 = $271M vs. Store 32 = $166M).  
- Useful for benchmarking best practices.  

📊 *Chart (Tableau)*:  
- ![Store Leaderboard](assets/charts/store_leaderboard.png)  

---

### 3. Holiday Impact  

- Average sales lift = **+7.8% during holiday weeks**.  
- Statistically significant (t-test p < 0.05).  

📊 *Charts (Python + Tableau)*:  
- ![Holiday Impact Bar](assets/charts/holiday_impact.png)  
- ![Holiday KPI Card](assets/charts/holiday_kpi.png)  

---

### 4. Seasonality by Month and Year  

- Highest sales in **November & December**.  
- Lows in **February and July**.  

📊 *Charts (Python + Tableau)*:  
- ![Seasonality by Month](assets/charts/seasonality_month.png)  
- ![Seasonality Heatmap](assets/charts/seasonality_heatmap.png)  

---

### 5. Correlation & Macroeconomic Drivers  

- **Fuel Price (–0.01)**, **Unemployment Rate (–0.11)**, and **CPI (–0.07)** show weak inverse relationships with sales.  
- Suggests Walmart’s demand is **resilient to macroeconomic pressure**, with holidays/seasonality driving more impact.  

📊 *Charts (Python)*:  
- ![Correlation Heatmap](assets/charts/correlation_heatmap.png)  
- ![Sales vs Unemployment](assets/charts/sales_vs_unemployment.png)  
- ![Sales vs Fuel Price](assets/charts/sales_vs_fuel.png)  

---

## Dashboard (Tableau)  

The Tableau dashboard consolidates KPIs and visuals:  

- **Filters**: Date Range, Holiday Flag  
- **KPI Cards**: Total Sales ($584M), Avg Sales ($999K), Holiday Impact (+7.8%)  
- **Charts**:  
  - Weekly Sales Over Time (line)  
  - Store Performance Leaderboard (bar)  
  - Sales vs Unemployment (scatter + trend)  
  - Seasonality by Month-Year (heatmap)  
  - Sales vs Fuel Prices (scatter)  

📊 *Screenshot (Tableau)*:  
- ![Tableau Dashboard](assets/charts/tableau_dashboard.png)  

---

## Recommendations  

1. **Holiday Optimization**: Increase inventory/promotions around holidays to leverage +7.8% lift.  
2. **Store Strategy**: Investigate top-performing stores (10, 27, 6) for best practices.  
3. **Forecasting Models**: Deploy rolling average or ARIMA models, accounting for holiday spikes.  
4. **Regional Insights**: Segment by geography if store metadata becomes available.  
5. **External Factors**: While macroeconomic drivers are weak, monitor CPI/fuel for lagged impacts.  

---

## Clarifying Questions & Assumptions  

- **Store metadata missing** → cannot segment by region.  
- **holidays_flag** only binary — does not distinguish between Thanksgiving, Christmas, etc.  
- No 2013+ data, limiting longer-term forecasting.  

---

## About  

This project demonstrates a **full-stack data workflow**:  

- **Excel** → Initial cleaning & pivot analysis  
- **SQL (BigQuery)** → Data wrangling, aggregations, schema documentation  
- **Python** → Exploratory analysis, statistics, correlation, and visualization  
- **Tableau** → Dashboard for stakeholders  

For more projects and my data journey, visit my [Portfolio](#).  
