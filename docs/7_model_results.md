# Model Results

This document summarizes the fare prediction modeling workflow and explains why
closed-form ridge regression is the selected model.

## 1. Objective

Predict `total_amount` for NYC taxi trips using engineered trip, route, time,
and borough features. The model is evaluated on true fare labels, with robust
capped-label diagnostics used to understand the impact of extreme fare values.

## 2. Data Split

| Split | Time Period | Role |
| --- | --- | --- |
| Train | All data before validation/test windows | Fit encoders, scalers, baselines, and model weights |
| Validation | Holdout period before final test months | Select the best model |
| Test | October to December 2024 | Final out-of-time evaluation |

All target encoders, standardization statistics, robust caps, and baseline maps
are fitted from the training split only.

## 3. Feature Strategy

- Numeric trip features: distance, duration, passenger count, month, weekday,
  and hour.
- Categorical route context: taxi color, pickup borough, and drop-off borough.
- Smoothed target encodings for categorical route context, fitted on training
  data only.
- Z-score standardization for continuous model features, fitted on training
  data only.

Excluded leakage-prone features:

- `fare_amount`
- `tolls_amount`

## 4. True-Label RMSE

| Model | Validation RMSE | Test RMSE | Notes |
| --- | ---: | ---: | --- |
| Baseline route-time average | 17.461 | 103.391 | Hierarchical averages with fallback levels |
| Model A closed-form ridge | 13.356 | 102.847 | Best validation model and selected artifact |
| Model B ridge gradient descent | 18.226 | 103.590 | Iterative ridge underperforms closed form |
| Model C Huber regression | 29.009 | 105.899 | More robust but weaker true-label fit |
| Model D fallback tree | 16.652 | 103.267 | Stronger than baseline validation, weaker than Model A |

## 5. Robust Diagnostics

| Model | Validation RMSE | Test RMSE |
| --- | ---: | ---: |
| Model A closed-form ridge | 12.579 | 12.613 |
| Model D fallback tree | 15.284 | 15.040 |

The gap between true-label and robust-label test RMSE indicates that extreme
fare values strongly affect the out-of-time test metric. Model A remains the
best choice because it has the strongest validation RMSE on the official
true-label target while also performing well on robust diagnostics.

## 6. Selection Rationale

Model A is selected because it provides the best balance of:

- **Accuracy:** lowest validation RMSE on true fare labels.
- **Simplicity:** closed-form ridge has fewer tuning knobs than iterative
  alternatives.
- **Repeatability:** model weights, target encodings, z-score stats, and
  metadata are persisted as explicit artifacts.
- **Operational fit:** the model is lightweight enough for notebook execution
  and can be migrated into a scheduled Databricks Workflow later.

## 7. Next Modeling Improvements

- Track model runs with MLflow.
- Add calendar features such as holiday and airport-period indicators.
- Compare against Spark MLlib linear regression and gradient-boosted trees on a
  sampled or feature-limited training set.
- Evaluate MAE alongside RMSE to reduce sensitivity to extreme fares.
- Add segment-level error analysis by taxi color, borough pair, and duration
  bin.
