# Demand Forecasting — Classical Time Series Approach

## Overview

This project builds a demand forecasting model for a multi-category retail dataset using classical time series methodology. The model forecasts daily product demand across five product categories using SARIMAX, with external variables such as promotions, discounts and epidemic flags incorporated as exogenous regressors.

---

## Dataset

- **Period:** January 2022 – January 2024
- **Records:** 76,000 rows (75,280 after feature engineering)
- **Categories:** Clothing, Electronics, Furniture, Groceries, Toys
- **Regions:** North, South, East, West
- **Stores:** 5 unique store IDs
- **Target Variable:** Daily Demand (units)

**Key Features:**
Price, Discount, Promotion, Competitor Pricing, Inventory Level, Weather Condition, Seasonality, Epidemic flag

---

## Project Pipeline

```
1. Data Loading & Inspection
2. Feature Engineering
3. Descriptive Statistics
4. Exploratory Data Analysis (EDA)
5. Stationarity Testing (ADF + KPSS)
6. ACF/PACF Analysis
7. SARIMAX Model Fitting
8. 30-Day Forecast (January 2024)
9. Evaluation — MAE, RMSE, MAPE
```

---

## Feature Engineering

| Group | Features |
|---|---|
| Calendar | Year, Month, Week, Quarter, Day of Week, Is Weekend, Month Start/End |
| Lag | Demand lag 1, 7, 14, 30 days |
| Rolling | 7/14/30-day rolling mean and std of demand |
| Price & Discount | Effective price, price gap vs competitor, discount tier |
| Interaction | Promotion x Discount, Epidemic x Category, Promotion x Season |
| Inventory | Stock gap, inventory coverage ratio, low stock flag |
| Cyclical | Sin/cos encoding of month, week, day of week |

---

## Stationarity Results

All five demand series were confirmed stationary by both ADF and KPSS tests — no differencing required (d = 0, D = 0).

| Category | ADF p-value | KPSS p-value | Result |
|---|---|---|---|
| Clothing | 0.0008 | 0.1000 | Stationary |
| Electronics | 0.0001 | 0.1000 | Stationary |
| Furniture | 0.0000 | 0.1000 | Stationary |
| Groceries | 0.0054 | 0.1000 | Stationary |
| Toys | 0.0000 | 0.1000 | Stationary |

---

## Model

**SARIMAX(1, 0, 1)(1, 0, 1, 7)**

| Parameter | Value | Reason |
|---|---|---|
| p | 1 | PACF cuts off after lag 1 |
| d | 0 | Series already stationary |
| q | 1 | Slow ACF decay |
| P | 1 | Seasonal AR |
| D | 0 | No seasonal differencing needed |
| Q | 1 | Seasonal MA |
| m | 7 | Weekly seasonality |

**Exogenous Variables:** Promotion, Epidemic, Discount

**Train Period:** January 2022 – December 2023
**Test Period:** January 2024 (30 days)

---

## Results

| Category | MAE | RMSE | MAPE % |
|---|---|---|---|
| Clothing | 231.10 | 267.24 | 11.69% |
| Electronics | 177.70 | 214.53 | 18.20% |
| Furniture | 176.83 | 221.58 | 15.51% |
| Groceries | 561.19 | 659.44 | 12.81% |
| Toys | 172.34 | 209.75 | 18.78% |

**Best Performer:** Clothing (MAPE 11.69%)
**Most Challenging:** Toys — sharp structural break in early January 2024

---

## Key Insights

- Lag features (lag_1 and lag_7) showed the strongest correlation with current demand, confirming high autocorrelation across all categories
- Promotions consistently increased demand across all categories
- Epidemic flag raised Groceries demand but suppressed Clothing and Toys
- All categories showed a post-New Year demand dip in the first 7–8 days of January 2024 — a structural seasonal effect not fully captured by the weekly seasonal component
- Groceries had the highest absolute error (MAE: 561) but lowest proportional error (MAPE: 12.81%) due to its much larger demand scale

---

## Tech Stack

- **Language:** Python 3
- **Libraries:** pandas, numpy, matplotlib, seaborn, statsmodels, scikit-learn

---

## Repository Structure

```
├── Demand_forecasting.ipynb             # Main analysis notebook
├── demand_forecasting.csv               # Raw dataset
├── Demand_forecasting_Report.docx       # Raw dataset
├── README.md                            # Project documentation
```

---

## Author

Soumi Maiti
MBA — Business Analytics & Systems Management, IISWBM (University of Calcutta)
B.Tech — Electronics & Communication Engineering
GitHub: github.com/soumi-maiti-14
