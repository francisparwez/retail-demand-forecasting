# Retail Demand Forecasting (Adaptive) — Project Summary

## Project

**Retail Demand Forecasting (Adaptive): Time Series Analysis, Temporal Feature Engineering & Multi-Model Forecast Comparison**

## Dataset

Kaggle Store Sales — Time Series Forecasting dataset from Corporación Favorita.

The main historical data is stored in `train.csv`, with supporting information from `stores.csv`, `oil.csv`, `holidays_events.csv`, and `transactions.csv`.

The future forecasting period is provided in `test.csv`.

The raw CSV files are excluded from GitHub because of their size.

## Project Goal

Build a complete retail demand forecasting workflow that accounts for trend, seasonality, autocorrelation, temporal dependencies, and future data leakage.

The project will compare statistical forecasting, Prophet, and XGBoost approaches using chronological validation and evaluate the final models using RMSE, MAE, and MAPE.

## Project Progress

### Part 01 — Time Series EDA

**Status:** ✅ Complete

The first stage analyzed the historical daily sales series and examined:

- overall sales trend
- weekly seasonality
- annual seasonality
- promotion effects
- holiday and event effects
- missing dates
- STL decomposition
- ADF stationarity testing
- KPSS stationarity testing
- first-difference stationarity
- ACF
- PACF

### Part 01 Findings

The daily sales series contains clear temporal structure, including weekly seasonality and strong autocorrelation.

Saturday and Sunday have higher average sales than most weekdays.

The monthly comparison and 365-day STL decomposition show longer-term seasonal and trend patterns.

The raw series is not stationary according to the ADF and KPSS results. First differencing produces a much more stationary series.

The ACF shows strong autocorrelation around lag 7, supporting the presence of weekly seasonality.

Promotion activity is positively associated with sales, with a sales-to-promotion correlation of approximately 0.575. This relationship should be treated as an association rather than proof of causation.

Observed holiday and event dates have different average sales behavior from other observed dates. The four missing Christmas dates are excluded from this comparison because there are no source sales records for those dates.

These findings will guide the temporal feature engineering and forecasting models in the next stages.

## Current Project Status

**Part 01 completed.**

The next stage will focus on temporal feature engineering and strict prevention of future data leakage.

## Project Files

### Data

- `data/raw/train.csv`
- `data/raw/test.csv`
- `data/raw/stores.csv`
- `data/raw/oil.csv`
- `data/raw/holidays_events.csv`
- `data/raw/transactions.csv`
- `data/raw/sample_submission.csv`

### Notebook

- `notebooks/retail_demand_forecasting.ipynb`

### Visualizations

- `images/1_daily_sales_over_time.png`
- `images/2_sales_by_day_of_week.png`
- `images/3_average_sales_by_month.png`
- `images/4_sales_vs_promotions.png`
- `images/5_holiday_sales_comparison.png`
- `images/6_stl_weekly.png`
- `images/7_stl_annual.png`
- `images/8_acf_daily_sales.png`
- `images/9_pacf_daily_sales.png`

## Planned Stages

1. Time Series EDA — Complete
2. Temporal Feature Engineering & Leakage Prevention
3. Statistical Forecasting
4. Prophet Forecasting
5. XGBoost Forecasting
6. Rolling-Origin Cross-Validation & Model Comparison
7. Final 16-Day Forecast
8. Business Reporting
