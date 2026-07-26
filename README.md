# 🛒 Walmart Store Sales — Demand Forecasting & Inventory Insights

Capstone project (Intellipaat Data Science internship) analyzing 3 years of weekly
sales across 45 Walmart stores to help inventory planning teams match supply with
demand — covering EDA, statistical drivers of sales, store segmentation, and a
12-week-ahead sales forecast per store.

📌 **Headline finding:** sales are driven far more by the retail calendar — specifically
Thanksgiving week — than by unemployment, temperature, or CPI.

![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-data%20wrangling-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-numerical-013243?logo=numpy&logoColor=white)
![scikit--learn](https://img.shields.io/badge/scikit--learn-clustering-F7931E?logo=scikitlearn&logoColor=white)
![statsmodels](https://img.shields.io/badge/statsmodels-forecasting-3776AB)
![SciPy](https://img.shields.io/badge/SciPy-statistics-8CAAE6?logo=scipy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-charts-11557C)
![Seaborn](https://img.shields.io/badge/Seaborn-visualization-4C72B0)
![Jupyter](https://img.shields.io/badge/Jupyter-notebook-F37626?logo=jupyter&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-dashboard-E34F26?logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-styling-1572B6?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-interactivity-F7DF1E?logo=javascript&logoColor=black)
![Chart.js](https://img.shields.io/badge/Chart.js-charts-FF6384?logo=chartdotjs&logoColor=white)

## 📊 Live dashboard

Open [`Walmart_Demand_Dashboard.html`](./Walmart_Demand_Dashboard.html) directly in a
browser — no server or build step required. It includes KPI cards, driver
correlations, the holiday-lift breakdown, store performance tiers, and an
interactive per-store 12-week forecast with confidence bands.

## ❓ Problem statement

A retail chain with multiple outlets is struggling to match inventory supply with
demand. Using the weekly sales data below, this project answers:

1. 📉 Does the unemployment rate affect weekly sales, and which stores suffer most?
2. 📅 Is there a seasonal trend in sales, and what drives it?
3. 🌡️ Does temperature affect weekly sales?
4. 💵 How does the Consumer Price Index (CPI) affect weekly sales across stores?
5. 🏆 Which stores perform best historically?
6. ⚠️ Which store performs worst, and how large is the gap vs. the top store?
7. 🔮 Can we forecast each store's sales for the next 12 weeks?

## 🗂️ Dataset

`Walmart_DataSet.csv` — 6,435 rows × 8 columns, 45 stores, weekly sales from
February 2010 to October 2012.

| Column | Description |
|---|---|
| Store | Store number (1–45) |
| Date | Week of sales |
| Weekly_Sales | Sales for that store in that week |
| Holiday_Flag | 1 if the week includes a major US holiday |
| Temperature | Regional temperature on the day of sale |
| Fuel_Price | Regional fuel cost |
| CPI | Consumer Price Index |
| Unemployment | Regional unemployment rate |

✅ No missing values or duplicate rows were found.

## 📁 Repository contents

| File | Description |
|---|---|
| 📓 `Walmart_Capstone_Analysis.ipynb` | Full analysis notebook — data quality checks, outlier analysis, EDA (drivers, seasonality, holiday breakdown, correlation heatmap), store-tier clustering, and per-store 12-week forecasting with validation |
| 📄 `Walmart_Capstone_Business_Report.docx` | Non-technical write-up of the same findings for stakeholders, with charts and recommendations |
| 🌐 `Walmart_Demand_Dashboard.html` | Interactive single-page dashboard (KPIs, charts, per-store forecast explorer) |
| 📈 `walmart_12week_forecast.csv` | 12-week-ahead forecast per store, with 80% confidence intervals (540 rows: 45 stores × 12 weeks) |
| 📋 `walmart_store_summary.csv` | Per-store ranking, performance tier, volatility, holiday sensitivity, and forecast validation accuracy |
| 🗃️ `Walmart_DataSet.csv` | Source dataset |

## 🔬 Methodology

**🧹 Data quality:** No missing values or duplicates. Outliers were screened per-store
using IQR on `Weekly_Sales` (flagging ~4.7% of weeks) but retained rather than
removed, since ~28% of flagged weeks are genuine holiday demand spikes rather than
data errors.

**🔍 EDA:**
- Pearson correlation (chain-wide and per-store) between `Weekly_Sales` and each of
  `Unemployment`, `Temperature`, `CPI`
- Seasonal decomposition (`statsmodels.seasonal_decompose`) on total weekly sales
- Holiday weeks broken out by specific event (Super Bowl, Labor Day, Thanksgiving,
  Christmas) to isolate which one actually drives the seasonal spike
- K-Means clustering of stores into High/Mid/Low-Volume tiers using average sales,
  volatility (coefficient of variation), and holiday sensitivity

**🔮 Forecasting:** Holt-Winters (triple exponential smoothing, additive trend + 52-week
additive seasonality) fit independently per store — chosen over SARIMA/regression
because each store has a strong annual pattern but a relatively short history
(~143 weeks) and no future values of the exogenous variables are available at
forecast time. Validated per store against a 12-week holdout and benchmarked against
a seasonal-naive baseline before trusting it for planning; confidence intervals are
generated via model simulation.

## 💡 Key findings

- 📉 **Unemployment, temperature, and CPI** all show weak or inconsistent correlation
  with weekly sales at the chain level — none is a reliable driver of demand
- 🦃 **Thanksgiving week** runs ~41% above an average week — by far the single biggest
  sales event in the calendar
- 🎄 The flagged "Christmas" week (week ending Dec 31) actually comes in *below*
  average — the real pre-Christmas rush happens in the unflagged weeks just before it
- 🏷️ Stores cluster into three performance tiers (High/Mid/Low-Volume) with different
  volatility and holiday-sensitivity profiles — a more actionable planning unit than
  a single sales ranking
- 🥇🆚🥉 The top store (#20) sold **8.1×** more than the weakest store (#33) over the
  3-year window — a gap too large to explain with the macro variables alone
- ✅ Holt-Winters forecasting beat a seasonal-naive baseline on 35 of 45 stores, with a
  median validation error (MAPE) of 3.0%

## 🛠️ Tools used

- 🐍 **Python** — pandas, NumPy, statsmodels, scikit-learn, SciPy, Matplotlib, Seaborn
- 📓 **Jupyter Notebook** — analysis and reporting
- 🌐 **HTML/CSS/JavaScript** with **Chart.js** — interactive dashboard

## ✅ Recommendations

1. 🦃 Build the sharpest inventory buffer specifically around Thanksgiving week, and
   treat the two weeks before December 31 — not that week itself — as the real
   Christmas peak
2. 🔮 Size orders off each store's 12-week forecast and confidence interval, not a
   single chain-wide number
3. 🏷️ Run High-Volume stores leaner (predictable demand); give Mid-Volume stores extra
   holiday stock (largest relative lift); give Low-Volume stores a larger relative
   safety buffer
4. 👀 Watch stores #38 and #44 for local unemployment swings — both are far more
   exposed than the rest of the chain
5. 🔁 Re-fit forecasts quarterly as new weeks of data arrive

# Author
**Mayank**

---
🎓 *Capstone project for the Intellipaat Data Science internship.*
