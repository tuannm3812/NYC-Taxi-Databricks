# NYC Taxi Databricks Analytics

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Databricks-FF3621?logo=databricks&logoColor=white" alt="Databricks">
  <img src="https://img.shields.io/badge/Engine-Apache%20Spark-E25A1C?logo=apachespark&logoColor=white" alt="Apache Spark">
  <img src="https://img.shields.io/badge/Language-Python-3776AB?logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Lakehouse-Delta%20Lake-00A1E0" alt="Delta Lake">
  <img src="https://img.shields.io/badge/Data-NYC%20Taxi-0F766E" alt="NYC Taxi">
  <img src="https://img.shields.io/badge/Status-Portfolio%20Ready-16A34A" alt="Portfolio Ready">
</p>

<p align="center">
  <img src="https://s.abcnews.com/images/Business/nyc-taxis-gty-rc-200220_hpMain.jpg" alt="New York City yellow taxis" width="860">
</p>

## Overview

NYC Taxi Databricks Analytics is a lakehouse analytics project built with Databricks, PySpark, and Delta Lake. It processes green and yellow taxi trip records, enriches trips with zone and borough attributes, produces operational and revenue insights, and trains predictive models from engineered trip features.

The repository is structured as a professional handover-ready project with a curated notebook, exported analysis report, implementation notes, and modular folders for future productionization.

## Key Capabilities

- Ingest green and yellow NYC taxi trip Parquet files into Databricks-managed storage.
- Load taxi zone lookup data and persist curated tables with Delta Lake.
- Harmonize schemas across taxi services and build a unified trip-level dataset.
- Engineer features for duration, distance, speed, time of travel, fare efficiency, and borough routes.
- Apply documented data quality rules with capped row removal.
- Analyze demand, revenue, tips, speed, and route patterns across month, weekday, hour, color, and borough pair.
- Train and evaluate machine learning models using scalable Spark ML workflows.

## Workflow Highlights

1. Configure Databricks catalog, schema, volume, and reusable table names.
2. Ingest raw taxi files and taxi zone reference data into managed lakehouse storage.
3. Standardize green and yellow taxi schemas into a shared trip model.
4. Apply quality filters and persist a borough-enriched Delta table.
5. Produce business-facing SQL analytics across demand, revenue, tips, and route behavior.
6. Train and compare baseline, ridge, Huber, and fallback tree-style models.

## Repository Structure

```text
.
|-- README.md
|-- .gitignore
|-- docs/
|   |-- databricks_setup.md
|   |-- notebook_summary.md
|   `-- project_handover.md
|-- notebooks/
|   `-- nyc_taxi_databricks_analytics.ipynb
|-- reports/
|   |-- nyc_taxi_databricks_analytics.html
|   `-- nyc_taxi_databricks_handover_report.pdf
`-- src/
    |-- ingestion/
    |-- transformation/
    |-- feature_engineering/
    `-- modeling/
```

## Primary Artifacts

- [notebooks/nyc_taxi_databricks_analytics.ipynb](notebooks/nyc_taxi_databricks_analytics.ipynb): Main Databricks notebook for ingestion, cleaning, enrichment, analytics, and machine learning.
- [reports/nyc_taxi_databricks_analytics.html](reports/nyc_taxi_databricks_analytics.html): Browser-friendly exported analysis report.
- [reports/nyc_taxi_databricks_handover_report.pdf](reports/nyc_taxi_databricks_handover_report.pdf): Project handover report.
- [docs/databricks_setup.md](docs/databricks_setup.md): Runtime, storage, and execution guidance.
- [docs/notebook_summary.md](docs/notebook_summary.md): Technical summary of the notebook workflow.
- [docs/project_handover.md](docs/project_handover.md): Editable source for the professional handover report.

## Databricks Execution

1. Clone this repository into a Databricks workspace using Repos.
2. Attach a cluster with Spark, Delta Lake, and Python support.
3. Confirm the Unity Catalog objects and volume paths in the notebook setup section.
4. Run [notebooks/nyc_taxi_databricks_analytics.ipynb](notebooks/nyc_taxi_databricks_analytics.ipynb) from top to bottom.
5. Regenerate the HTML report after changing notebook logic or outputs.

## Suggested Repository Description

Databricks and PySpark lakehouse analytics project for NYC green and yellow taxi trip data, including Delta Lake processing, borough-level business insights, machine learning, and professional handover artifacts.

## Future Improvements

- Move reusable notebook logic into tested modules under `src/`.
- Add environment and dependency automation for Databricks jobs.
- Add data quality checks for schema drift and anomaly detection.
- Schedule the pipeline as a Databricks Workflow.
- Track model experiments and metrics with MLflow.

## License

For academic and portfolio use unless otherwise specified by the data provider or institution.
