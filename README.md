# Weather Trend Forecasting

This project analyzes global weather observations and builds multiple forecasting models to predict temperature trends over time. The work is located in `weather_trend_forecasting.ipynb`, which walks through preprocessing, exploratory analysis, anomaly detection, forecasting, and geospatial visualization using the `GlobalWeatherRepository.csv` dataset.

## Project Overview

The notebook is structured as an end-to-end weather analytics workflow:

- Preprocesses the raw weather dataset, removes duplicates, and parses timestamps.
- Engineers calendar-based and cyclical time features such as year, month, day of week, day of year, `doy_sin`, and `doy_cos`.
- Performs exploratory data analysis on temperature and other numeric weather variables.
- Detects unusual observations with both Isolation Forest and Z-score-based anomaly flags.
- Forecasts temperature using three approaches: Random Forest, ARIMA, and XGBoost.
- Tunes model hyperparameters with Optuna.
- Combines model predictions with a simple stacked ensemble.
- Explores climate, environmental, and spatial patterns through visualizations and feature importance analysis.

## Files

- `weather_trend_forecasting.ipynb`: main notebook containing the full analysis and forecasting workflow.
- `GlobalWeatherRepository.csv`: source dataset with global weather, air quality, and location fields.
- `requirements.txt`: Python dependencies currently tracked in the repo.

## Setup

Create and activate a virtual environment, then install the dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Dataset

The notebook expects a CSV named `GlobalWeatherRepository.csv` in the project root. The dataset includes fields such as:

- Location metadata: `country`, `location_name`, `latitude`, `longitude`, `timezone`
- Time fields: `last_updated_epoch`, `last_updated`
- Weather measurements: `temperature_celsius`, `wind_kph`, `pressure_mb`, `humidity`, `cloud`, `precip_mm`, `visibility_km`, `uv_index`, `gust_kph`
- Air quality features: `air_quality_Carbon_Monoxide`, `air_quality_Ozone`, `air_quality_Nitrogen_dioxide`, `air_quality_Sulphur_dioxide`, `air_quality_PM2.5`, `air_quality_PM10`
- Astronomical features: `sunrise`, `sunset`, `moonrise`, `moonset`, `moon_phase`, `moon_illumination`

## Notebook Workflow

### 1. Preprocessing

The notebook loads the CSV, parses `last_updated` as a datetime column, checks for missing values, and removes duplicate rows.

### 2. Feature Engineering

Time-based features are extracted from `last_updated`, then cyclical encodings are created to help models learn seasonal structure. A small set of specific dates is also removed before modeling.

### 3. Exploratory Data Analysis

The analysis summarizes numeric variables and visualizes monthly temperature behavior to understand broad seasonal trends.

### 4. Anomaly Detection

Two anomaly signals are created:

- `anomaly_iso` from Isolation Forest
- `anomaly_z` from a temperature Z-score threshold

These are merged into `anomaly_combined`, which is later used as a modeling feature.

### 5. Forecasting

The notebook resamples the data to daily frequency, forward-fills gaps, creates 14 lag features for temperature, and splits the data into training and test sets.

It then trains and tunes:

- Random Forest Regressor
- ARIMA
- XGBoost Regressor

### 6. Ensemble Modeling

Predictions from the three base models are combined with a linear regression meta-model to produce a stacked forecast and evaluate ensemble performance with MAE.

### 7. Additional Analysis

The final sections explore:

- Global average temperature trends over time
- Regional climate trends for sampled locations
- Relationships between temperature and air pollution
- Random Forest and XGBoost feature importance
- Geographic temperature patterns with Plotly
- Country-level average temperature comparisons

## Notes

- The repository’s main analysis is notebook-based, so results depend on cell execution order and the local Python environment.
- The current workflow focuses on temperature forecasting, with additional environmental and spatial analysis built around the same dataset.
