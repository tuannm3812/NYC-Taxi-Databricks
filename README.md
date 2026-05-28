# NYC Taxi Databricks Analytics

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Databricks-FF3621?logo=databricks&logoColor=white" alt="Databricks">
  <img src="https://img.shields.io/badge/Engine-Apache%20Spark-E25A1C?logo=apachespark&logoColor=white" alt="Apache Spark">
  <img src="https://img.shields.io/badge/Language-Python-3776AB?logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Lakehouse-Delta%20Lake-00A1E0" alt="Delta Lake">
  <img src="https://img.shields.io/badge/Data-NYC%20Taxi-0F766E" alt="NYC Taxi">
</p>

<p align="center">
  <img src="https://s.abcnews.com/images/Business/nyc-taxis-gty-rc-200220_hpMain.jpg" alt="New York City yellow taxis" width="860">
</p>

## 1. Overview

This project builds a **Databricks lakehouse analytics workflow** for NYC green
and yellow taxi trips. It processes raw trip records at large scale, standardizes
taxi schemas, enriches trips with borough metadata, answers operational business
questions, and trains Spark-friendly models to predict `total_amount`.

The project is now positioned as a **personal data engineering and analytics
portfolio project**. The notebook remains the executable source of truth, while
the exported HTML report and documentation capture the evidence, decisions, and
model results.

## 2. Goal

The main goal is to show an end-to-end big data workflow:

- ingest and harmonize **green** and **yellow** NYC taxi records;
- engineer reliable trip, speed, distance, time, and fare features;
- apply transparent quality filters without over-removing data;
- save a curated **Delta Lake** table for analytics and modeling;
- answer borough-level demand, revenue, tip, and duration-efficiency questions;
- compare baseline and machine learning models for trip fare prediction;
- diagnose model error by business segments so improvement work is targeted.

## 3. Key Metrics

| Area | Result |
| --- | ---: |
| Raw trips processed | **991,467,464** |
| Clean trips retained | **974,672,551** |
| Rows removed by quality filters | **16,794,913** |
| Removal rate | **1.69%** |
| Taxi zones loaded | **265** |
| Trips with tips | **62.94%** |
| Tipped trips with tips >= $15 | **0.83%** |
| Top 2024 revenue route | **Manhattan -> Manhattan** |
| Top 2024 route revenue share | **62.45%** |
| Best base model | **Closed-form ridge regression** |
| Best improved model | **Route-calibrated Model E** |
| Baseline validation RMSE | **17.461** |
| Best validation RMSE | **13.356** |
| Model E test MAE | **4.197** |
| Model E test bias | **-0.533** |
| Operational filtered RMSE | **7.762** |

## 4. Progress

Completed:

- Built the Databricks notebook workflow from ingestion through analytics and
  modeling.
- Standardized different taxi schemas into one trip-level analytical model.
- Persisted the curated borough-enriched dataset as a partitioned Delta table.
- Produced SQL-based business outputs for demand, revenue, tips, route flows,
  and duration efficiency.
- Trained and compared a route-time baseline, closed-form ridge regression,
  gradient-descent ridge, Huber regression, and fallback tree-style scoring.
- Added model artifact persistence so the selected model can be reused for
  faster diagnostics.
- Added segment-level model error analysis by taxi color, month, duration bin,
  and borough-pair route.
- Added a second model-improvement notebook with validation-fitted route
  calibration, enhanced ridge features, and operational tail-anomaly reporting.

Current finding:

Route-calibrated **Model E** reduces practical prediction error substantially:
MAE improves from **9.851** to **4.197**, and bias improves from **-9.373** to
**-0.533**. The full raw RMSE remains high because **0.0162%** of test rows
contain extreme anomaly-like fares, including a maximum `total_amount` of
**335,550.94**. After isolating those rows, the operational filtered RMSE is
**7.762**.

## 5. Key Findings

- **Data quality is controlled:** the cleaning rules remove only **1.69%** of
  raw rows while filtering invalid timestamps, unrealistic durations,
  unrealistic distances, and impossible speeds.
- **Revenue is concentrated:** in 2024, **Manhattan -> Manhattan** contributes
  **62.45%** of route-pair revenue, and the top routes dominate total revenue.
- **Short trips are operationally attractive:** trips under five minutes have
  the highest estimated revenue per hour, while longer trips generate larger
  fares but lower hourly efficiency.
- **Model E is the strongest current model:** validation-fitted route
  calibration removes most systematic underprediction while keeping the model
  simple and explainable.
- **Aggregate RMSE hides tail risk:** normal-trip error is much lower than raw
  RMSE suggests; only **1,751** of **10.8M** test rows are flagged as
  operational anomalies, but they dominate squared error.

## 6. Technical Skills Demonstrated

- **Databricks:** Workspace, Unity Catalog, Volumes, serverless/general compute,
  notebook execution, and exported reports.
- **Spark and Delta Lake:** PySpark DataFrames, Spark SQL, schema harmonization,
  large-scale joins, partitioned Delta tables, and reusable model artifacts.
- **Analytics engineering:** monthly KPI tables, route revenue ranking, tip
  analysis, duration-efficiency metrics, and borough-pair analysis.
- **Machine learning:** time-based splits, train-only preprocessing, target
  encoding, z-score standardization, baseline modeling, ridge regression,
  validation residual calibration, robust labels, and tail diagnostics.
- **Project communication:** professional README, technical docs, Databricks
  HTML output, and handover-style reporting.

## 7. Repository Map

```text
.
|-- README.md
|-- docs/
|   |-- 0_coding_standards.md
|   |-- 1_project_brief.md
|   |-- 2_databricks_setup.md
|   |-- 3_notebook_summary.md
|   |-- 4_project_handover.md
|   |-- 5_architecture.md
|   |-- 6_data_quality.md
|   |-- 7_model_results.md
|   `-- 8_next_work.md
|-- notebooks/
|   |-- 1_nyc_taxi_databricks_analytics.ipynb
|   `-- 2_model_improvement.ipynb
|-- reports/
|   |-- nyc_taxi_databricks_analytics.html
|   `-- nyc_taxi_databricks_handover_report.pdf
`-- src/
    |-- feature_engineering/
    |-- ingestion/
    |-- modeling/
    `-- transformation/
```

The project is intentionally **notebook-first**. The `src/` folders document
future module boundaries if the workflow is converted into Databricks Jobs or a
scheduled production pipeline.

## 8. Documentation

The main executable workflow is
[notebooks/1_nyc_taxi_databricks_analytics.ipynb](notebooks/1_nyc_taxi_databricks_analytics.ipynb).
The next modeling stage starts in
[notebooks/2_model_improvement.ipynb](notebooks/2_model_improvement.ipynb).
Detailed setup, architecture, data quality, model results, and next-work notes
are maintained in `docs/`.
