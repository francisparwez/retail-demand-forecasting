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

### Part 02 — Temporal Feature Engineering & Leakage Prevention

**Status:** ✅ Complete

The second stage transformed the daily sales series into forecasting features using historical information only.

The engineered features include:

- lag features for 1, 7, 14, and 28 days
- rolling means for 7, 14, and 28 days
- rolling standard deviations for 7, 14, and 28 days
- calendar features
- weekly Fourier terms
- annual Fourier terms
- holiday and event indicators
- promotion indicators

Rolling features were calculated after shifting the sales series by one day to prevent the current day's sales from being included in its own features.

The feature table uses a continuous calendar index so that lag periods represent actual calendar days.

Leakage checks confirmed that the lag and rolling features use only previous sales observations.

The engineered dataset will be used in the later forecasting and model comparison stages.

### Part 03 — Forecasting Model Implementation & Tuning

**Status:** ✅ Complete

Three forecasting approaches were implemented and tuned:

- SARIMA
- Prophet
- XGBoost

SARIMA was configured with a 7-day seasonal period to capture weekly demand patterns.

Prophet was configured with weekly and yearly seasonality and tuned using a chronological validation search.

XGBoost was trained using the temporal features created in Part 02 and tuned using chronological TimeSeriesSplit folds.

The preliminary tuning stage did not use the final test period.

The next stage will perform formal rolling-origin cross-validation so that the three models can be compared using the same time-series validation process.

### Part 04 — Rolling-Origin Cross-Validation

**Status:** ✅ Complete

Three expanding-window validation folds of 28 days each were used.

Model tuning was completed on an earlier historical period that ended before the first rolling-origin validation fold.

SARIMA and Prophet were fitted only on historical observations available before each validation period.

XGBoost validation forecasts were generated recursively so actual future validation sales were not used to create lag or rolling features.

The models were evaluated using RMSE, MAE, and MAPE.

The final mean cross-validation results were:

- XGBoost — RMSE: 68419.94, MAE: 52306.91, MAPE: 5.95%
- SARIMA — RMSE: 86348.65, MAE: 62350.61, MAPE: 6.99%
- Prophet — RMSE: 98565.13, MAE: 79710.42, MAPE: 9.56%

XGBoost achieved the strongest average performance across all three metrics.

No shuffling was used and the final test period remained untouched.

## Current Project Status

**Part 04 completed.**

The next stage will focus on final model selection and the 16-day forecast.

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
- `images/10_lag_feature_example.png`
- `images/11_rolling_features.png`
- `images/12_fourier_features.png`
- `images/13_feature_correlation.png`
- `images/14_sarima_validation_forecast.png`
- `images/15_prophet_validation_forecast.png`
- `images/16_xgboost_validation_forecast.png`
- `images/17_xgboost_feature_importance.png`
- `images/18_cv_rmse_by_fold.png`
- `images/19_cv_mae_by_fold.png`
- `images/20_cv_mape_by_fold.png`
- `images/21_cv_model_comparison.png`
- `images/22_final_cv_fold_forecasts.png`

## Planned Stages

1. Time Series EDA — Complete
2. Temporal Feature Engineering & Leakage Prevention — Complete
3. Forecasting Model Implementation & Tuning — Complete
4. Rolling-Origin Cross-Validation — Complete
5. Model Comparison & Selection
6. Final 16-Day Forecast
7. Prediction Intervals & Error Analysis
8. Business Reporting
