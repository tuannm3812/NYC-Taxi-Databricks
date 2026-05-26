# Project Handover

This document combines project handover notes and evidence from the successful
Databricks v3 run.

## 1. Summary

NYC Taxi Databricks Analytics is a notebook-first Databricks lakehouse project
for green and yellow taxi trip data. It ingests raw Parquet files, standardizes
taxi-service schemas, enriches trips with zone and borough metadata, generates
business analytics, and trains fare prediction models from engineered features.

## 2. Technical Stack

- Databricks Workspace for notebook execution.
- Unity Catalog and Volumes for governed storage.
- PySpark DataFrames for distributed transformation.
- Spark SQL for analytical queries.
- Delta Lake for curated table and model artifact persistence.
- NumPy for lightweight closed-form and iterative model calculations.

## 3. Pipeline Flow

1. Configure Spark timezone and Unity Catalog paths.
2. Optionally download raw green and yellow taxi Parquet files.
3. Load the NYC taxi zone lookup and validate unique `LocationID` values.
4. Harmonize green/yellow source schemas into a shared trip schema.
5. Engineer trip duration, distance, speed, time, and fare-efficiency features.
6. Apply transparent data quality filters and report retained rows.
7. Enrich pickup and drop-off locations with borough and zone metadata.
8. Persist the curated `taxi_trips_cleaned_borough` Delta table.
9. Run stakeholder-facing analytics queries.
10. Train, evaluate, and persist selected model artifacts.

## 4. Analytics Coverage

- Monthly demand, revenue, passenger count, weekday, and hour patterns.
- Green vs yellow taxi duration, distance, and speed distributions.
- Borough-pair route demand by taxi color, month, weekday, and hour.
- Top 2024 borough-pair revenue routes and revenue share.
- Tip participation and high-tip trip share.
- Speed and kilometers-per-dollar by trip-duration band.
- Revenue per hour by trip-duration band for the driver recommendation.

## 5. Modeling Coverage

- Time-based train, validation, and test splits.
- Hierarchical route-time baseline with fallback levels.
- Train-only robust label clipping.
- Train-only smoothed target encoding.
- Train-only z-score standardization.
- Closed-form ridge regression selected for artifact persistence.
- Optional diagnostics for feature correlations and coefficients.
- Long-running diagnostics such as VIF are kept out of the default run and
  summarized separately from the earlier diagnostic output.

## 6. Operational Notes

- Keep `RUN_PROFILE_PREVIEWS = False` for normal runs.
- Keep `RUN_MODEL_DIAGNOSTICS = False` unless preparing appendix evidence.
- Regenerate the HTML report after a successful Databricks run.
- Do not commit raw data, model checkpoints, or duplicate notebook exports.
- Move stable helpers into `src/` only when scheduling this as a production job.

## 7. Run Evidence

Source reviewed: `nyc_taxi_databricks_analytics_v3.html`.

Data scale:

- Green taxi rows: 83,484,688.
- Yellow taxi rows: 907,982,776.
- Raw total rows: 991,467,464.
- Taxi zones loaded: 265.
- Final clean rows: 974,672,551.
- Removed rows: 16,794,913.
- Removed percentage: 1.69%.

The cleaning logic is conservative: it removes 1.69% of raw rows while
preserving enough data for stable analytics and model training.

Business findings:

- Manhattan-to-Manhattan was the largest 2024 borough-pair revenue route,
  contributing 62.45% of 2024 revenue in the top-route output.
- Queens-to-Manhattan was second, contributing 15.21%.
- 62.94% of trips included tips.
- Among tipped trips, 0.83% had tips of at least $15.
- Green taxis had slightly higher average speed than yellow taxis in the
  cleaned output: 20.51 km/h vs 18.91 km/h.

Model results on true labels:

| Model | Validation RMSE | Test RMSE |
| --- | ---: | ---: |
| Baseline | 17.461 | 103.391 |
| Model A closed-form ridge | 13.356 | 102.847 |
| Model B ridge gradient descent | 18.226 | 103.590 |
| Model C Huber | 29.009 | 105.899 |
| Model D fallback tree | 16.652 | 103.267 |

Model A had the best validation RMSE and slightly beat the baseline on the
October-December 2024 test split. Robust RMSE was much lower and more stable,
which indicates extreme true-label fares strongly affect test RMSE.

## 8. Diagnostics Decision

Earlier diagnostics included correlation, coefficient, and VIF analysis. VIF
values were modest, with the largest values around distance and duration
features. However, VIF required extra pairwise correlation work over a large
Spark table.

The refined notebook keeps diagnostics opt-in through:

```python
RUN_MODEL_DIAGNOSTICS = False
```

This keeps the default run focused on core evidence and model comparison while
preserving optional diagnostic capability for deeper review.

## 9. Recommended Next Work

- Rerun the refined notebook in Databricks after material source changes and
  replace the HTML report.
- Add MLflow tracking for model parameters, metrics, and artifacts.
- Add schema drift checks and source file manifests.
- Convert the notebook into a Databricks Workflow with separate tasks for
  ingestion, transformation, analytics, and modeling.
- Add segment-level error analysis by taxi color, borough pair, and duration
  bin.
