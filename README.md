# Demand Forecasting Project
## Project overview
This repository contains a forecasting project that develops, compares and evaluates time-series forecasting models (ETS, (S)ARIMA(X), XGBoost) to predict monthly SKU demand for a retail chain in the Netherlands. The work covers data preparation, exploratory analysis, time-series clustering, model selection and backtesting with expanding-window validation. The goal is to produce robust monthly forecasts for inventory planning and to identify which modelling approach works best at SKU level.
## Key facts
- Forecast frequency: monthly (derived from original daily sales data).
- Time period: April 2018 — December 2022 (original transaction-level data aggregated to months).
- Target: demand quantity per SKU (monthly).
- Exogenous features used: Consumer confidence, Willingness to buy, Derived CPI, Unemployment rate, Covid case, Covid death.
- Models evaluated: ETS, (S)ARIMA(X), XGBoost.
- Model selection: auto_arima for ARIMA orders, GridSearchCV for XGBoost hyperparameters.
- Validation: expanding-window validation using TimeSeriesSplit (1-step ahead forecasts).
- Metrics: MSE and MAPE.
## Project framework
### 1. Data Aggregation and Time-series Clustering
- Raw daily sales per SKU collected and aggregated to monthly demand series (month start frequency).
- External variables (exog) collected manually from public/official sources and merged with monthly series.
- Elbow method to suggest number of clusters; clusters were used to reduce per-SKU modelling effort and pick medoids (representative SKUs).
- DTW (Dynamic Time Warping) distance, K-means style clustering for grouping SKU with similar demand patterns.
### 2. Exploratory Data Analysis
- Seasonal decomposition, ACF / PACF plots, stationarity checks.
- Basic feature engineering with lag features (lag_1, lag_2, lag_3).
### 3. Model Training 
- ETS: fitted using additive error/trend/seasonality (A, A, A) as baseline.
- (S)ARIMA(X): pmdarima.auto_arima() used on each series to determine (p,d,q)(P,D,Q,m).
- XGBoost: feature-based model trained on lag+exog features. Hyperparameter grid used are n_estimators, max_depth, and learning_rate
- Expanding-window validation (starting training window = 36 months) with 1-step-ahead forecasts for each validation split.
- Aggregated MSE and MAPE reported per model per SKU.
### 4. Reporting and Visualization
- Comparison charts for MSE (line) and MAPE (bar) per SKU.
- Saved plots and a results CSV summarising best parameters, MSE and MAPE per SKU/model.
## Key results
- ARIMA produced the lowest MSE for 5 out of 6 evaluated medoid SKUs (strong baseline).
- XGBoost delivered lowest MAPE for SKU 1563 and SKU 1596 — indicating it better captured relative forecasting accuracy for those SKUs (likely because of exogenous influence).
- XGBoost achieved the best performance for SKU 4271 on both MSE and MAPE (suggests XGBoost learned patterns not captured by ARIMA/ETS).
