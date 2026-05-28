# Notebook Summary

Notebook: `notebooks/1_nyc_taxi_databricks_analytics.ipynb`

This document summarizes the executable notebook flow. Project goals are
summarized in `docs/1_project_brief.md`, and run evidence is captured in
`docs/4_project_handover.md`.

## 1. Purpose

The notebook implements a Databricks lakehouse workflow for NYC green and
yellow taxi trip data. It builds a curated borough-enriched Delta table, answers
business questions with Spark SQL, and trains lightweight predictive models from
engineered trip features.

## 2. Runtime Modes

The configuration section controls optional work:

- `RUN_DOWNLOADS`: downloads raw Parquet files into the configured volume.
- `ALLOW_RUNTIME_PIP_INSTALL`: permits installing `gdown` inside the notebook.
- `OVERWRITE_TABLES`: controls whether Delta tables/artifacts are overwritten.
- `RUN_PROFILE_PREVIEWS`: enables raw schema and sample-row displays.
- `RUN_MODEL_TRAINING`: fits baseline, Model A, and optional candidates.
- `RUN_CANDIDATE_MODELS`: trains Model B, Model C, and Model D during a full
  model run.
- `LOAD_MODEL_ARTIFACTS`: loads persisted Model A weights, target encoders,
  z-score statistics, and metadata instead of refitting them.
- `SAVE_MODEL_PREDICTIONS`: persists scored Model A test predictions.
- `RUN_MODEL_DIAGNOSTICS`: enables extra fare and feature diagnostics.
- `RUN_SEGMENT_ERROR_ANALYSIS`: profiles Model A errors by business segment.

Normal runs should keep previews and diagnostics disabled to reduce runtime.
The notebook still reports raw row counts by taxi color because they are useful
scale and validation checkpoints.

For fast diagnostics after the first full training run, use
`RUN_MODEL_TRAINING = False` with `LOAD_MODEL_ARTIFACTS = True`. This skips
baseline fitting and candidate model training while preserving Model A scoring
and segment error analysis.

## 3. Workflow

1. **Lakehouse Build**
   - Configure imports, Spark timezone, Unity Catalog objects, and volume paths.
   - Load raw green and yellow taxi Parquet files.
   - Register the taxi zone lookup as a Delta table.
   - Harmonize service-specific schemas into one trip table.
   - Engineer duration, distance, speed, temporal, and fare-efficiency fields.
   - Apply data quality filters and report row retention.
   - Persist the borough-enriched curated Delta table.

2. **Business Analytics**
   - Summarize monthly demand and revenue.
   - Compare green and yellow taxi distributions.
   - Build a borough route-time demand grid.
   - Rank 2024 borough-pair revenue routes.
   - Measure tip participation and high-tip share.
   - Compare speed, distance efficiency, and revenue per hour by duration band.

3. **Machine Learning**
   - Create time-based train, validation, and test splits.
   - Build a hierarchical route-time baseline.
   - Fit robust label clipping from training data only.
   - Fit target encoders and standardization stats from training data only.
   - Train ridge, gradient-descent, Huber, and fallback tree-style models.
   - Compare RMSE and persist selected Model A artifacts.
   - Load saved Model A artifacts for faster repeat diagnostics when training
     is disabled.
   - Profile Model A errors by taxi color, duration bin, month, and borough
     route pair.

## 4. Design Notes

- The row-retention audit is the primary full-data quality checkpoint.
- Raw DataFrame previews are optional because they are not needed for every run.
- Model diagnostics are optional because pairwise correlation checks are useful
  for review but expensive on large Spark tables.
- VIF analysis from the previous successful run is summarized in
  `docs/4_project_handover.md`; it is not part of the default rerun path.
- The notebook stays self-contained; `src/` is reserved for future
  productionized modules.

## 5. Companion Docs

- `docs/5_architecture.md`: end-to-end lakehouse architecture and
  productionization path.
- `docs/6_data_quality.md`: cleaning rules, row-retention evidence, and
  modeling implications.
- `docs/7_model_results.md`: model comparison, selection rationale, and next
  modeling improvements.
- `docs/8_next_work.md`: focused plan for Model E, route-aware features,
  temporal features, and productionization.

## 6. Reviewer Artifacts

- `reports/nyc_taxi_databricks_analytics.html`: exported notebook report.
- `reports/nyc_taxi_databricks_handover_report.pdf`: formal handover report.
- `docs/4_project_handover.md`: editable handover source.
