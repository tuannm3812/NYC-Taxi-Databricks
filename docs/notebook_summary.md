# Notebook Summary

Notebook: `notebooks/nyc_taxi_databricks_analytics.ipynb`

## Purpose

The notebook implements an end-to-end NYC taxi analytics workflow in Databricks. It converts raw green and yellow taxi trip files into a curated analytical dataset, answers route and revenue questions, and trains predictive models from engineered trip features.

## Workflow

1. **Lakehouse Setup and Ingestion**
   - Configure Spark timezone for NYC-local temporal features.
   - Define Unity Catalog and volume paths.
   - Download green and yellow taxi Parquet files into Databricks storage.
   - Register taxi zone lookup data as Delta.

2. **Data Preparation**
   - Harmonize green and yellow taxi schemas.
   - Union trips into a consistent dataset.
   - Derive duration, distance, speed, temporal, and fare-efficiency features.
   - Apply data quality filters while retaining visibility into row removal.
   - Enrich pickup and drop-off locations with borough metadata.

3. **Business Analytics**
   - Summarize monthly taxi activity.
   - Compare descriptive statistics by taxi color.
   - Analyze pickup-to-dropoff borough flows by month, day, and hour.
   - Identify high-revenue borough-pair routes.
   - Evaluate tip percentages and fare efficiency.
   - Compare travel speed and distance economics across duration bands.

4. **Machine Learning**
   - Split datasets for training, validation, and testing.
   - Build a route-time baseline model.
   - Apply target encoding without validation or test leakage.
   - Standardize numeric features.
   - Assemble feature vectors and train Spark ML models.
   - Summarize model performance and project conclusions.

## Handover Notes

- The notebook is the executable source of truth.
- The HTML report is the review artifact for stakeholders who do not need to run Databricks.
- The PDF report is retained as a formal handover document.
- The `src/` folders provide a migration path from exploratory notebook workflow to reusable production modules.
