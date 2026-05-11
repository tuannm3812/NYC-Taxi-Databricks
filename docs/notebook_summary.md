# Notebook Summary

Source notebook: `Assignment_2_ManhTuan_Nguyen.ipynb`

## Confirmed Topic

The notebook is about **NYC taxi trip analytics**, not weather forecasting. It works with green and yellow taxi datasets, taxi zones, Delta tables, feature engineering, business analytics, and machine learning.

## Main Sections

1. **Data Ingestion & Cleaning**
   - Environment and Databricks storage setup.
   - Green and yellow taxi Parquet ingestion.
   - Taxi zone lookup loading into Delta.
   - Schema harmonisation across taxi colors.
   - Derived trip features such as duration, distance, speed, and revenue efficiency.
   - Cleaning with a documented removal cap.
   - Borough enrichment and final table persistence.

2. **Business Questions**
   - Year-month trip summary.
   - Descriptive statistics by taxi color.
   - Pickup-to-dropoff borough grids by month, day of week, and hour.
   - Top pickup-to-dropoff revenue share for 2024.
   - Tip percentage analysis.
   - Duration bin analysis for average speed and kilometers per dollar.

3. **Machine Learning**
   - Dataset splitting.
   - Baseline model.
   - Target encoding.
   - Feature standardization.
   - Feature assembly.
   - Model training and conclusion.

## Professional Handover Notes

- The notebook should remain the executable source of truth.
- The HTML report should be regenerated whenever notebook logic or outputs change.
- The PDF handover report should summarize assumptions, business findings, model performance, and limitations.
- Reusable code can be progressively moved from notebook cells into the `src/` modules when the project evolves beyond assignment submission.
