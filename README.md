# Retail Demand Forecasting (Adaptive): Time Series Analysis, Temporal Feature Engineering & Multi-Model Forecast Comparison

## Project Overview

This project builds an end-to-end retail demand forecasting solution using the Kaggle Store Sales — Time Series Forecasting dataset from Corporación Favorita.

The project focuses on the parts that make time series forecasting different from standard machine learning, including trend, seasonality, autocorrelation, temporal dependencies, and future data leakage.

The final workflow will compare a statistical forecasting model, Prophet, and XGBoost using leakage-safe temporal features and rolling-origin cross-validation.

## Project Goal

The main goal is to forecast retail demand accurately while keeping the chronological structure of the data.

The final project will:

- analyze historical sales patterns
- engineer temporal forecasting features
- compare three forecasting approaches
- validate models using expanding-window cross-validation
- evaluate models using RMSE, MAE, and MAPE
- produce a final 16-day forecast with prediction intervals
- recommend a model based on forecasting performance and business considerations

## Dataset

The project uses the Kaggle Store Sales — Time Series Forecasting dataset.

The main historical sales data is stored in `train.csv`.

Supporting files include:

- `stores.csv`
- `oil.csv`
- `holidays_events.csv`
- `transactions.csv`

`test.csv` contains the future period used for the final forecasting stage.

The raw CSV files are kept locally and are excluded from GitHub because of their size.

## Part 01 — Time Series EDA

### Status

✅ Complete

The first stage focuses on understanding the historical retail sales series before building forecasting models.

The analysis includes:

- data and date-range checks
- daily sales aggregation
- missing-date analysis
- overall sales trend
- weekly seasonality
- annual seasonality
- promotion effects
- holiday and event effects
- STL decomposition using 7-day and 365-day seasonal periods
- ADF stationarity testing
- KPSS stationarity testing
- first-difference stationarity analysis
- ACF analysis
- PACF analysis

### Key Findings

The aggregated daily sales series contains clear temporal structure.

Average sales vary considerably across the days of the week, with Saturday and Sunday showing higher average sales than most weekdays.

The monthly comparison and the 365-day STL decomposition show longer-term seasonal and trend patterns in addition to the strong weekly cycle.

The raw sales series is not stationary. The ADF test produced a p-value of approximately 0.0887, while the KPSS test produced a statistic of approximately 5.7310 with a p-value of 0.01.

After first differencing, the ADF test strongly rejected the unit-root hypothesis and the KPSS test no longer indicated non-stationarity. This suggests that differencing may be useful for later statistical forecasting methods.

The ACF shows strong autocorrelation, including a strong relationship around lag 7, supporting the presence of weekly seasonality. The PACF also shows meaningful relationships at several early lags.

Promotion activity is positively associated with daily sales. The correlation between total daily sales and the number of products on promotion is approximately 0.575. This suggests that promotion information may be useful for forecasting, although correlation alone does not establish causation.

Observed holiday and event dates also show different average sales behavior from other observed dates. The four missing Christmas dates are not included in this comparison because no sales records are present for those dates in the source data.

### Visualizations

![Daily Sales Over Time](images/1_daily_sales_over_time.png)

![Average Sales by Day of Week](images/2_sales_by_day_of_week.png)

![Average Sales by Month](images/3_average_sales_by_month.png)

![Sales vs Promotions](images/4_sales_vs_promotions.png)

![Holiday and Event Sales Comparison](images/5_holiday_sales_comparison.png)

![Weekly STL Decomposition](images/6_stl_weekly.png)

![Annual STL Decomposition](images/7_stl_annual.png)

![ACF of Daily Sales](images/8_acf_daily_sales.png)

![PACF of Daily Sales](images/9_pacf_daily_sales.png)

## Part 02 — Temporal Feature Engineering & Leakage Prevention

### Status

✅ Complete

The second stage converts the historical daily sales series into forecasting features while making sure that future sales information is never used when creating historical features.

The features created include:

- lag features for 1, 7, 14, and 28 days
- 7-, 14-, and 28-day rolling means
- 7-, 14-, and 28-day rolling standard deviations
- calendar features
- weekly Fourier terms
- annual Fourier terms
- holiday and event indicators
- promotion indicators

The rolling calculations use the previous observations only. Each rolling feature is created after shifting the sales series by one day, which prevents the current day's sales from leaking into its own features.

The feature engineering also uses a continuous calendar index so that a lag of 7 represents seven calendar days rather than seven observed rows.

### Leakage Check

Lag and rolling features were checked directly against their expected previous-value calculations.

The final feature table does not use future target values to construct historical training features.

### Visualizations

![Lag Feature Example](images/10_lag_feature_example.png)

![Rolling Features](images/11_rolling_features.png)

![Fourier Features](images/12_fourier_features.png)

![Feature Correlation](images/13_feature_correlation.png)

## Part 03 — Forecasting Model Implementation & Tuning

### Status

✅ Complete

Three forecasting approaches were implemented and tuned:

- SARIMA
- Prophet
- XGBoost

### SARIMA

SARIMA was used as the statistical forecasting approach with a seasonal period of 7 days to capture the weekly pattern identified during the EDA.

A small parameter search was used to select the strongest configuration based on AIC.

### Prophet

Prophet was configured with weekly and yearly seasonality.

The model was tuned using a chronological validation search over trend and seasonality parameters.

### XGBoost

XGBoost was trained as a regression model using the temporal features created in Part 02.

The model used lag features, rolling statistics, calendar variables, Fourier terms, holiday and event indicators, and promotion information.

XGBoost hyperparameters were tuned using chronological `TimeSeriesSplit` folds without shuffling.

### Model Evaluation

The preliminary validation results are used to confirm that the models are working and to support tuning.

The final comparison of SARIMA, Prophet, and XGBoost will be performed using rolling-origin cross-validation in the next stage.

The test dataset remains untouched.

### Visualizations

![SARIMA Validation Forecast](images/14_sarima_validation_forecast.png)

![Prophet Validation Forecast](images/15_prophet_validation_forecast.png)

![XGBoost Validation Forecast](images/16_xgboost_validation_forecast.png)

![XGBoost Feature Importance](images/17_xgboost_feature_importance.png)

## Part 04 — Rolling-Origin Cross-Validation

### Status

✅ Complete

The three tuned forecasting models were evaluated using expanding-window rolling-origin cross-validation.

An earlier historical period was used for model tuning, ending before the first rolling-origin validation fold. This kept the validation periods separate from hyperparameter selection.

Three chronological validation folds were used, with each fold covering 28 consecutive days.

The training window expanded through time, and no random shuffling was used.

SARIMA and Prophet were fitted using only historical observations available before each validation period.

XGBoost validation forecasts were generated recursively so that actual future validation sales were never used to create lag or rolling features.

The models were evaluated using RMSE, MAE, and MAPE.

### Cross-Validation Results

| Model   | Mean RMSE | Mean MAE | Mean MAPE |
| ------- | --------: | -------: | --------: |
| XGBoost |  68419.94 | 52306.91 |     5.95% |
| SARIMA  |  86348.65 | 62350.61 |     6.99% |
| Prophet |  98565.13 | 79710.42 |     9.56% |

XGBoost achieved the lowest mean RMSE, MAE, and MAPE across the rolling-origin validation folds.

The final test dataset was not used during cross-validation.

### Cross-Validation Visualizations

![RMSE Across Rolling Validation Folds](images/18_cv_rmse_by_fold.png)

![MAE Across Rolling Validation Folds](images/19_cv_mae_by_fold.png)

![MAPE Across Rolling Validation Folds](images/20_cv_mape_by_fold.png)

![Mean Rolling-Origin Model Comparison](images/21_cv_model_comparison.png)

![Final Rolling-Origin Validation Fold](images/22_final_cv_fold_forecasts.png)

## Project Progress

| Part | Stage                                             | Status      |
| ---- | ------------------------------------------------- | ----------- |
| 01   | Time Series EDA                                   | ✅ Complete |
| 02   | Temporal Feature Engineering & Leakage Prevention | ✅ Complete |
| 03   | Forecasting Model Implementation & Tuning         | ✅ Complete |
| 04   | Rolling-Origin Cross-Validation                   | ✅ Complete |
| 05   | Model Comparison & Selection                      | Planned     |
| 06   | Final 16-Day Forecast                             | Planned     |
| 07   | Prediction Intervals & Error Analysis             | Planned     |
| 08   | Business Reporting                                | Planned     |

## Project Structure

```text
retail-demand-forecasting/
├── data/
│   └── raw/
│       ├── holidays_events.csv
│       ├── oil.csv
│       ├── sample_submission.csv
│       ├── stores.csv
│       ├── test.csv
│       ├── train.csv
│       └── transactions.csv
├── notebooks/
│   └── retail_demand_forecasting.ipynb
├── images/
│   ├── 1_daily_sales_over_time.png
│   ├── 2_sales_by_day_of_week.png
│   ├── 3_average_sales_by_month.png
│   ├── 4_sales_vs_promotions.png
│   ├── 5_holiday_sales_comparison.png
│   ├── 6_stl_weekly.png
│   ├── 7_stl_annual.png
│   ├── 8_acf_daily_sales.png
│   ├── 9_pacf_daily_sales.png
│   ├── 10_lag_feature_example.png
│   ├── 11_rolling_features.png
│   ├── 12_fourier_features.png
│   ├── 13_feature_correlation.png
│   ├── 14_sarima_validation_forecast.png
│   ├── 15_prophet_validation_forecast.png
│   ├── 16_xgboost_validation_forecast.png
│   ├── 17_xgboost_feature_importance.png
│   ├── 18_cv_rmse_by_fold.png
│   ├── 19_cv_mae_by_fold.png
│   ├── 20_cv_mape_by_fold.png
│   ├── 21_cv_model_comparison.png
│   └── 22_final_cv_fold_forecasts.png
├── .gitignore
├── README.md
├── SUMMARY.md
└── requirements.txt
```

## Tools & Libraries

- Python
- pandas
- NumPy
- Matplotlib
- Seaborn
- statsmodels
- scikit-learn
- XGBoost
- Prophet
