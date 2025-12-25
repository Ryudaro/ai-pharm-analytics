# System Architecture

## Overview

AI-Pharm Analytics is a local-first demand forecasting system designed
for pharmaceutical and pharmacy sales data without mandatory use of
cloud services.

The system architecture is focused on:
- reproducibility of forecasting results,
- transparency of forecasting logic,
- practical applicability in real-world pharmacy environments.

The system is built as a set of logically separated components, each
responsible for a specific stage of the forecasting process.

---

## Core components

### 1. Data layer

The data layer is responsible for preparing input data for forecasting models.

Core responsibilities:
- loading sales data from local sources (CSV, Excel),
- validating data structure and consistency,
- basic data cleaning and time series preprocessing,
- transforming data into formats suitable for model training.

The data layer operates on structured tabular time series and is
independent of specific forecasting models.

---

### 2. Modeling layer

The modeling layer contains forecasting models and their corresponding wrappers.

Core responsibilities:
- training models on historical data,
- generating forecasts for a specified horizon,
- managing model parameters and configurations,
- providing a unified interface across different modeling approaches.

Supported approaches include:
- classical time series models (e.g., ARIMA / SARIMA),
- machine learning methods (e.g., XGBoost).

Each model is isolated and can be extended or replaced without modifying
other system components.

---

### 3. Evaluation layer

The evaluation layer is responsible for assessing forecast quality.

Core responsibilities:
- calculating forecast error metrics,
- comparing results across different models,
- producing interpretable performance indicators.

Evaluation relies on metrics relevant to pharmacy practice, such as
MAE, RMSE, and MAPE, allowing forecast accuracy to be interpreted in
practical and meaningful terms.

---

### 4. Forecasting workflow (pipeline control)

This component manages the forecasting process as a unified sequence
of steps.

In practice, it:
- defines the execution order of all stages (data → models → forecast → evaluation),
- fixes parameters and execution settings,
- ensures reproducibility of results,
- serves as a single entry point for running forecasts and experiments.

A single pipeline run corresponds to one complete forecasting scenario,
including model training, forecast generation, and quality evaluation.

This approach supports both research-oriented experiments and
regular operational forecasting in pharmacy organizations.

---

## Execution flow

A typical system workflow consists of the following steps:

1. Loading and validating sales data  
2. Time series preprocessing  
3. Splitting data into training and test sets  
4. Training selected forecasting models  
5. Generating forecasts for a specified horizon  
6. Calculating forecast accuracy metrics  
7. Saving results and execution parameters  

---

## Design principles

- local-first execution without mandatory cloud infrastructure;
- clear separation of responsibilities between system components;
- reproducibility and transparency of forecasting workflows;
- focus on real-world pharmacy use cases rather than purely academic tasks.

---

## Notes

The architecture is designed to support future extensions, including:
- adding new forecasting models,
- integrating alternative data sources,
- connecting to pharmacy accounting systems.

At the same time, the core forecasting logic and overall system structure
are intended to remain stable.
