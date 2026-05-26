# NYC Taxi Databricks Analytics

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Databricks-FF3621?logo=databricks&logoColor=white" alt="Databricks">
  <img src="https://img.shields.io/badge/Engine-Apache%20Spark-E25A1C?logo=apachespark&logoColor=white" alt="Apache Spark">
  <img src="https://img.shields.io/badge/Language-Python-3776AB?logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Lakehouse-Delta%20Lake-00A1E0" alt="Delta Lake">
  <img src="https://img.shields.io/badge/Data-NYC%20Taxi-0F766E" alt="NYC Taxi">
</p>

Databricks lakehouse project for NYC green and yellow taxi trip data. The
notebook ingests raw Parquet files, builds curated Delta tables, produces
borough-level analytics, and trains Spark-based fare prediction models from
engineered trip features.

This repository is designed as a viewer-facing portfolio project: the notebook
is the executable source, while the exported reports and documentation explain
the analytical decisions and handover path.

## 1. Project Highlights

- Processes nearly 1 billion raw taxi trips in Databricks using PySpark and
  Delta Lake.
- Standardizes green and yellow taxi schemas into a shared analytical model.
- Applies explicit data quality rules and retains 974.7M clean trips from the
  latest exported run, removing 1.69% of raw rows.
- Enriches trips with NYC taxi zone and borough metadata from 265 locations.
- Analyzes demand, revenue, tips, speed, duration, distance efficiency, and
  borough-to-borough route behavior.
- Builds a modeling workflow with time-based splits, hierarchical baselines,
  target encoding, z-score standardization, ridge regression, Huber regression,
  and fallback tree-style scoring.

## 2. Technical Skills

- **Cloud analytics:** Databricks Workspace, Unity Catalog, Volumes, managed
  Delta tables.
- **Distributed data engineering:** PySpark DataFrames, Spark SQL, schema
  harmonization, large-scale joins, aggregation, and persistence.
- **Lakehouse design:** raw volume paths, curated tables, reusable artifact
  paths, and governed table naming.
- **Data quality:** timestamp bounds, distance/duration/fare filters, row
  retention measurement, and borough enrichment checks.
- **Analytics engineering:** route-level SQL metrics, monthly summaries,
  revenue share analysis, tip behavior, and operational speed/distance KPIs.
- **Machine learning:** train/validation/test splitting, target encoding,
  feature standardization, baseline modeling, closed-form ridge regression,
  gradient descent, Huber loss, and model artifact persistence.
- **Project communication:** exported HTML report, handover PDF, setup notes,
  and notebook summary documentation.

## 3. Repository Structure

```text
.
|-- README.md
|-- docs/
|   |-- 0_coding_standards.md
|   |-- 1_assignment_brief.md
|   |-- 2_databricks_setup.md
|   |-- 3_notebook_summary.md
|   `-- 4_project_handover.md
|-- notebooks/
|   `-- nyc_taxi_databricks_analytics.ipynb
|-- reports/
|   |-- nyc_taxi_databricks_analytics.html
|   `-- nyc_taxi_databricks_handover_report.pdf
`-- src/
    |-- feature_engineering/
    |-- ingestion/
    |-- modeling/
    `-- transformation/
```

The project is intentionally notebook-first. The `src/` folders are lightweight
placeholders for future productionization and should stay small until reusable
job modules are introduced.

## 4. Key Artifacts

- [notebooks/nyc_taxi_databricks_analytics.ipynb](notebooks/nyc_taxi_databricks_analytics.ipynb) -
  executable Databricks notebook.
- [reports/nyc_taxi_databricks_analytics.html](reports/nyc_taxi_databricks_analytics.html) -
  exported analysis report for reviewers.
- [reports/nyc_taxi_databricks_handover_report.pdf](reports/nyc_taxi_databricks_handover_report.pdf) -
  formal handover report.
- [docs/0_coding_standards.md](docs/0_coding_standards.md) - notebook,
  runtime, documentation, and git hygiene standards.
- [docs/1_assignment_brief.md](docs/1_assignment_brief.md) - assignment
  requirements summarized as project tasks.
- [docs/2_databricks_setup.md](docs/2_databricks_setup.md) - Databricks runtime,
  storage, and execution guidance.
- [docs/3_notebook_summary.md](docs/3_notebook_summary.md) - technical workflow
  summary.
- [docs/4_project_handover.md](docs/4_project_handover.md) - editable
  handover source with v3 run evidence.

## 5. How to Run

1. Clone this repository into a Databricks Git folder.
2. Open `notebooks/nyc_taxi_databricks_analytics.ipynb`.
3. Attach a cluster with Spark, Python, Delta Lake, and Unity Catalog support.
4. Confirm the configuration section near the top of the notebook:

   ```python
   CATALOG = "workspace"
   SCHEMA = "bde"
   VOLUME = "nyc_taxi"
   RUN_DOWNLOADS = True
   ALLOW_RUNTIME_PIP_INSTALL = True
   OVERWRITE_TABLES = True
   RUN_PROFILE_PREVIEWS = False
   RUN_MODEL_DIAGNOSTICS = False
   ```

5. Upload `taxi_zone_lookup.csv` to:

   ```text
   /Volumes/<catalog>/<schema>/<volume>/taxi_zone_lookup.csv
   ```

6. Run all notebook cells from top to bottom.
7. Export a refreshed HTML report after material logic or output changes.

The notebook writes these main outputs:

```text
<catalog>.<schema>.taxi_zone_lookup
<catalog>.<schema>.taxi_trips_cleaned_borough
/Volumes/<catalog>/<schema>/<volume>/models/model_a_ridge_v1
```

## 6. Current Lessons

- **Data quality rules should be visible, not hidden.** The latest exported run
  removed 16.8M rows, or 1.69% of the raw dataset, while preserving a large
  clean analytical base.
- **Borough context makes the data business-readable.** Zone enrichment turns
  trip-level records into interpretable pickup/dropoff flows for route,
  revenue, and operations analysis.
- **Manhattan remains the dominant revenue engine.** In the latest exported
  2024 route summary, Manhattan-to-Manhattan trips account for the largest
  revenue share.
- **Notebook-first work still needs production habits.** Configuration flags,
  train-only encoders, clean artifact paths, and cleared outputs make the
  notebook easier to review and rerun.
- **Optional diagnostics should stay opt-in.** Raw previews and feature
  diagnostics are useful for development, but disabling them keeps normal
  Databricks reruns focused on the core evidence.
- **VIF is useful but expensive.** Earlier diagnostics showed no severe
  multicollinearity, so the default notebook now skips VIF-style diagnostics.
- **Train-only baselines matter.** The latest run fits baseline route-time
  averages from the training split only, so validation RMSE is not inflated by
  validation-label leakage.

## 7. Next Work

- Rerun the refined notebook in Databricks after material source changes and
  replace the exported HTML report with the successful full-run output.
- Schedule the workflow as a Databricks Workflow with explicit task boundaries
  for ingestion, transformation, analytics, and modeling.
- Add MLflow tracking for model parameters, metrics, and artifacts.
- Move stable notebook functions into tested `src/` modules once the workflow
  graduates from portfolio notebook to reusable production pipeline.
- Add schema drift checks, source file manifests, and automated data quality
  assertions.
- Publish a concise model results page under `docs/` after the corrected model
  section is rerun.

## 8. Git Hygiene

Do not commit raw taxi data, Databricks working directories, local model
checkpoints, notebook checkpoints, Python caches, or ad hoc experiment dumps.
Commit the clean source notebook and lightweight reviewer artifacts by default.
For assignment submission, an executed Databricks notebook can be exported and
included alongside the HTML report.

## 9. License

For academic and portfolio use unless otherwise specified by the data provider
or institution.
