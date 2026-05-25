# Project Handover

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
  summarized separately from the successful v2 output.

## 6. Operational Notes

- Keep `RUN_PROFILE_PREVIEWS = False` for normal reruns.
- Keep `RUN_MODEL_DIAGNOSTICS = False` unless preparing appendix evidence.
- Regenerate the HTML report after a successful Databricks run.
- Do not commit raw data, model checkpoints, or duplicate notebook exports.
- Move stable helpers into `src/` only when scheduling this as a production job.

## 7. Recommended Next Work

- Rerun the refined notebook in Databricks and replace the HTML report.
- Add MLflow tracking for model parameters, metrics, and artifacts.
- Add schema drift checks and source file manifests.
- Convert the notebook into a Databricks Workflow with separate tasks for
  ingestion, transformation, analytics, and modeling.
- Publish a concise model results page after the corrected full run completes.
