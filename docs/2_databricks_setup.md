# Databricks Setup

## 1. Runtime

Use a Databricks Runtime with Spark, Python, Delta Lake, and Unity Catalog
support. A single-node cluster is enough for code review and smaller reruns;
autoscaling is recommended for full-scale execution over the taxi dataset.

The notebook sets:

```python
spark.conf.set("spark.sql.session.timeZone", "America/New_York")
```

This keeps hour, weekday, and month features aligned with NYC taxi operations.

## 2. Storage Layout

Default Unity Catalog values:

```python
CATALOG = "workspace"
SCHEMA = "bde"
VOLUME = "nyc_taxi"
```

Default volume paths:

```text
/Volumes/workspace/bde/nyc_taxi/green
/Volumes/workspace/bde/nyc_taxi/yellow
/Volumes/workspace/bde/nyc_taxi/taxi_zone_lookup.csv
/Volumes/workspace/bde/nyc_taxi/models/model_a_ridge_v1
```

Update the configuration cell if your workspace uses different names.

## 3. Required Input

Upload the NYC taxi zone lookup file before running the notebook:

```text
/Volumes/<catalog>/<schema>/<volume>/taxi_zone_lookup.csv
```

The green and yellow taxi Parquet files can be downloaded by the notebook when
`RUN_DOWNLOADS = True`. Existing raw files are skipped so repeated reruns do not
download the same large files again.

## 4. Runtime Flags

Recommended values for a normal full rerun:

```python
RUN_DOWNLOADS = True
ALLOW_RUNTIME_PIP_INSTALL = True
OVERWRITE_TABLES = True
RUN_PROFILE_PREVIEWS = False
RUN_MODEL_TRAINING = True
RUN_CANDIDATE_MODELS = True
LOAD_MODEL_ARTIFACTS = False
SAVE_MODEL_PREDICTIONS = True
RUN_MODEL_DIAGNOSTICS = False
RUN_SEGMENT_ERROR_ANALYSIS = True
```

Use `RUN_PROFILE_PREVIEWS = True` only when inspecting raw schemas or samples.
Use `RUN_MODEL_DIAGNOSTICS = True` only when preparing appendix evidence.

For faster model diagnostics after the Model A artifacts have been created,
reuse saved encoders, z-score statistics, metadata, and weights:

```python
RUN_MODEL_TRAINING = False
RUN_CANDIDATE_MODELS = False
LOAD_MODEL_ARTIFACTS = True
RUN_SEGMENT_ERROR_ANALYSIS = True
```

This mode still rebuilds validation/test scoring frames from the curated table,
but skips baseline fitting and candidate model training.

## 5. Execution Workflow

1. Clone this repository into a Databricks Git folder.
2. Open `notebooks/nyc_taxi_databricks_analytics.ipynb`.
3. Attach compatible compute.
4. Confirm catalog, schema, volume, and runtime flags.
5. Confirm the taxi zone lookup CSV exists in the configured volume.
6. Run all cells in order.
7. Export a refreshed HTML report after any material logic or output change.

## 6. Expected Outputs

```text
<catalog>.<schema>.taxi_zone_lookup
<catalog>.<schema>.taxi_trips_cleaned_borough
/Volumes/<catalog>/<schema>/<volume>/models/model_a_ridge_v1
<catalog>.<schema>.model_a_test_predictions
```

## 7. Troubleshooting

- If `CREATE CATALOG`, `CREATE SCHEMA`, or `CREATE VOLUME` fails, ask a
  Databricks admin to create the objects or grant the required permissions.
- If `gdown` cannot be installed, install it on the cluster or set
  `ALLOW_RUNTIME_PIP_INSTALL = False` after making the package available.
- If the notebook rerun is slow, keep previews and diagnostics disabled and
  confirm the source files already exist in the configured volume.
