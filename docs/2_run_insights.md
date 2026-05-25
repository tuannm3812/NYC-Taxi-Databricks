# Run Insights

Source reviewed:
`nyc_taxi_databricks_analytics_v2.html`

## 1. Data Scale

- Green taxi rows: 83,484,688.
- Yellow taxi rows: 907,982,776.
- Raw total rows: 991,467,464.
- Taxi zones loaded: 265.
- Final clean rows: 974,672,551.
- Removed rows: 16,794,913.
- Removed percentage: 1.69%.

The cleaning logic satisfies the assignment constraint that no more than 10% of
the dataset should be removed.

## 2. Business Findings

- Manhattan-to-Manhattan was the largest 2024 borough-pair revenue route,
  contributing 62.45% of 2024 revenue in the top-route output.
- Queens-to-Manhattan was second, contributing 15.21%.
- 62.94% of trips included tips.
- Among tipped trips, 0.83% had tips of at least $15.
- Green taxis had slightly higher average speed than yellow taxis in the
  cleaned output: 20.51 km/h vs 18.91 km/h.
- The previous duration-bin output showed `60+ mins` had the highest
  kilometers per dollar. The refined notebook now also calculates revenue per
  hour so the driver recommendation can focus on income rather than only
  distance efficiency.

## 3. Model Results

Validation RMSE on true labels:

- Baseline: 16.948.
- Model A closed-form ridge: 13.356.
- Model B ridge gradient descent: 18.226.
- Model C Huber: 29.009.
- Model D fallback tree: 16.652.

Test RMSE on true labels:

- Baseline: 103.391.
- Model A closed-form ridge: 102.847.
- Model B ridge gradient descent: 103.590.
- Model C Huber: 105.899.
- Model D fallback tree: 103.267.

Model A had the best validation RMSE and slightly beat the baseline on the
October-December 2024 test split. Robust RMSE was much lower and more stable,
which indicates extreme true-label fares strongly affect test RMSE.

## 4. Diagnostics Decision

The v2 run included correlation, coefficient, and VIF diagnostics. VIF values
were modest, with the largest values around distance and duration features.
However, VIF required extra pairwise correlation work over a large Spark table.

The refined notebook keeps diagnostics opt-in through:

```python
RUN_MODEL_DIAGNOSTICS = False
```

This keeps the default run focused on required assignment outputs and model
comparison while preserving optional diagnostic capability for appendix work.

## 5. Code Adjustments From Review

- Keep raw counts by taxi color because the assignment explicitly requires
  green/yellow row counts.
- Keep raw previews disabled by default to reduce runtime.
- Keep VIF-style diagnostics disabled by default because they are not required
  for the main deliverable and can run for a long time.
- Add revenue-per-hour to duration-bin analysis to support the driver income
  recommendation directly.
