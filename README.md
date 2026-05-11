# NYC Taxi Databricks Analytics

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Databricks-FF3621?logo=databricks&logoColor=white" alt="Databricks">
  <img src="https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Spark-PySpark-E25A1C?logo=apachespark&logoColor=white" alt="PySpark">
  <img src="https://img.shields.io/badge/Data-NYC%20Taxi-0F766E" alt="NYC Taxi">
  <img src="https://img.shields.io/badge/Status-Assignment%20Ready-16A34A" alt="Status">
</p>

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&height=180&text=NYC%20Taxi%20Databricks%20Analytics&fontAlign=50&fontAlignY=35&color=0:111827,45:0F766E,100:F59E0B&fontColor=ffffff" alt="NYC Taxi Databricks Analytics banner">
</p>

## Repository Purpose

This repository contains a professional handover package for **Assignment 2 - 94693 Big Data Engineering - Spring 2025**. The project analyzes NYC green and yellow taxi trip data in Databricks using PySpark, Delta tables, feature engineering, business analytics, and machine learning.

## Project Scope

- Ingest green and yellow NYC taxi trip Parquet files into Databricks storage.
- Load and persist taxi zone lookup data as Delta.
- Harmonize schemas across taxi colors and derive trip-level features.
- Clean data with documented quality controls and a capped removal policy.
- Enrich trips with pickup and drop-off borough information.
- Answer business questions across month, hour, weekday, borough pair, revenue, tips, speed, and distance efficiency.
- Train and evaluate machine learning models using engineered taxi trip features.

## Repository Structure

```text
.
|-- README.md
|-- .gitignore
|-- docs/
|   |-- databricks_setup.md
|   `-- notebook_summary.md
|-- notebooks/
|   `-- Assignment_2_ManhTuan_Nguyen.ipynb
|-- reports/
|   |-- Assignment_2_ManhTuan_Nguyen.html
|   `-- BDE_Assignment_2_ManhTuan_Nguyen_HandoverReport.pdf
`-- src/
    |-- ingestion/
    |-- transformation/
    |-- feature_engineering/
    `-- modeling/
```

## Main Artifacts

- [notebooks/Assignment_2_ManhTuan_Nguyen.ipynb](notebooks/Assignment_2_ManhTuan_Nguyen.ipynb): Databricks notebook containing ingestion, cleaning, analytics, and ML workflow.
- [reports/Assignment_2_ManhTuan_Nguyen.html](reports/Assignment_2_ManhTuan_Nguyen.html): Exported notebook report for review.
- [reports/BDE_Assignment_2_ManhTuan_Nguyen_HandoverReport.pdf](reports/BDE_Assignment_2_ManhTuan_Nguyen_HandoverReport.pdf): Final handover report.
- [docs/databricks_setup.md](docs/databricks_setup.md): Databricks setup and repo workflow notes.
- [docs/notebook_summary.md](docs/notebook_summary.md): Notebook structure and content summary.

## Databricks Setup

1. Clone this repository into a Databricks workspace using Repos.
2. Use a Databricks Runtime that supports PySpark, Delta Lake, and notebook execution.
3. Confirm Unity Catalog objects and volume paths used by the notebook are available:
   - Catalog: `workspace`
   - Schema: `bde`
   - Volume: `assignment2`
4. Run the notebook from top to bottom after confirming data access permissions.
5. Export a refreshed HTML report after major notebook changes.

## Suggested Commit Strategy

Use small commits grouped by function:

- `chore(repo): scaffold Databricks assignment structure`
- `docs(readme): document NYC taxi analytics workflow`
- `docs(setup): add Databricks execution guidance`
- `feat(notebook): add assignment notebook artifact`
- `docs(report): add exported report and handover PDF`

## Recommended Repository Name

`NYC-Taxi-Databricks`

## Recommended Repository Description

Databricks and PySpark analytics project for NYC green and yellow taxi trip data, including ingestion, Delta Lake processing, business insights, machine learning, and assignment handover artifacts.

## License

For academic use unless otherwise specified by the university or course requirements.
