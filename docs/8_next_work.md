# Next Work

## 1. Modeling Improvements

- Promote **Model E route calibration** into the main model narrative.
- Add route indicators for **airport**, **EWR**, and **Unknown borough** trips
  to address remaining high-fare special cases.
- Add `year` or month-index features to capture long-term fare changes if they
  improve operational metrics.
- Compare full raw, capped, operational-filtered, robust, MAE, and bias metrics
  after each change.
- Keep segment-level diagnostics by taxi color, month, duration bin, and
  borough-pair route as the main model review lens.
- Keep `workspace.bde.model_e_tail_review` as the audit table for outlier and
  anomaly follow-up.

## 2. Notebook Direction

Use numbered notebooks for the active workflow:

- `notebooks/1_nyc_taxi_databricks_analytics.ipynb`: current end-to-end
  lakehouse, analytics, and baseline modeling workflow.
- `notebooks/2_model_improvement.ipynb`: focused experiments for calibration,
  route-aware features, and temporal features.

Promote stable experiments from notebook 2 back into notebook 1 only after they
improve validation and test diagnostics clearly.

## 3. Production Path

- Add MLflow tracking for model metrics, parameters, and persisted artifacts.
- Add schema drift checks and source file manifests.
- Convert stable stages into a Databricks Workflow with separate tasks for
  ingestion, transformation, analytics, training, and diagnostics.
- Move reusable helper logic into `src/` modules when the workflow becomes a
  scheduled pipeline rather than a notebook-first project.
