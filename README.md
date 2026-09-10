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

## Project Progress

| Part | Stage                                              | Status      |
| ---- | -------------------------------------------------- | ----------- |
| 01   | Time Series EDA                                    | ✅ Complete |
| 02   | Temporal Feature Engineering & Leakage Prevention  | Planned     |
| 03   | Statistical Forecasting                            | Planned     |
| 04   | Prophet Forecasting                                | Planned     |
| 05   | XGBoost Forecasting                                | Planned     |
| 06   | Rolling-Origin Cross-Validation & Model Comparison | Planned     |
| 07   | Final 16-Day Forecast                              | Planned     |
| 08   | Business Reporting                                 | Planned     |

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
│   └── 9_pacf_daily_sales.png
├── reports/
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
