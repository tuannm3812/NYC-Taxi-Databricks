# Model Results

This document summarizes the fare prediction modeling workflow, the base ridge
model, and the later Model E calibration improvement.

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

Model A was selected as the best base model because it provides the best balance
of:

- **Accuracy:** lowest validation RMSE on true fare labels.
- **Simplicity:** closed-form ridge has fewer tuning knobs than iterative
  alternatives.
- **Repeatability:** model weights, target encodings, z-score stats, and
  metadata are persisted as explicit artifacts.
- **Operational fit:** the model is lightweight enough for notebook execution
  and can be migrated into a scheduled Databricks Workflow later.

The notebook can now reuse these artifacts by setting
`RUN_MODEL_TRAINING = False` and `LOAD_MODEL_ARTIFACTS = True`. This mode
skips baseline fitting and candidate model training, then reloads Model A
weights, target encoders, z-score statistics, metadata, and robust-cap values
for scoring and diagnostics.

## 7. Model Improvement Results

The second notebook, `notebooks/2_model_improvement.ipynb`, starts from the
curated Delta table and saved Model A artifacts. It tests validation-fitted
calibration and route-aware enhanced ridge features without rebuilding the full
lakehouse.

Model E route calibration is the strongest current model. It applies residual
corrections learned from the validation window and shrunk by route-pair support.

| Model | True RMSE | MAE | Bias | Robust RMSE |
| --- | ---: | ---: | ---: | ---: |
| Model A artifact | 102.847 | 9.851 | -9.373 | 12.613 |
| Model F enhanced ridge | 102.453 | 5.255 | -3.535 | 8.941 |
| Model E global calibration | 102.419 | 5.308 | -0.227 | 8.613 |
| Model E color calibration | 102.419 | 5.308 | -0.228 | 8.614 |
| **Model E route calibration** | **102.284** | **4.197** | **-0.533** | **7.117** |

The improvement is clearest in practical error metrics: MAE drops by more than
half, and average bias is close to zero. Raw RMSE improves only slightly because
it remains dominated by a very small number of extreme fare records.

## 8. Operational Metric View

The model-improvement notebook reports both transparent full metrics and an
operational view that isolates extreme anomaly-like rows.

| Metric View | Rows | RMSE | MAE | Bias |
| --- | ---: | ---: | ---: | ---: |
| Full raw metric | 10,829,246 | 102.284 | 4.197 | -0.533 |
| Full capped at 1000 | 10,829,246 | 7.902 | 4.166 | -0.502 |
| Operational filtered | 10,827,495 | 7.762 | 4.151 | -0.486 |

Only 1,751 rows, or 0.0162% of the test set, are flagged as operational
anomalies. They include three rows above the extreme amount threshold and a set
of very high fare-per-minute or high-fare Manhattan internal records. These
rows dominate squared error despite having little impact on MAE.

The notebook persists:

- `workspace.bde.model_e_test_predictions`
- `workspace.bde.model_f_test_predictions`
- `workspace.bde.model_e_tail_review`

## 9. Segment Error Analysis

The v4 Databricks run includes a `3.9 Segment Error Analysis` section for the
selected Model A test predictions. It reports:

- error by taxi color;
- error by trip-duration bin;
- error by test month;
- highest-error pickup-to-drop-off borough pairs with enough test trips.

Each segment table includes:

| Metric | Meaning |
| --- | --- |
| `trip_count` | Number of test trips in the segment |
| `avg_actual` | Average true `total_amount` |
| `avg_predicted` | Average Model A prediction |
| `bias` | Average prediction error, calculated as prediction minus actual |
| `mae` | Mean absolute error |
| `rmse` | Root mean squared error |

This analysis turns aggregate RMSE into a more useful diagnostic view. It can
show whether model error is concentrated in long trips, specific borough flows,
or particular months in the out-of-time test period.

When `SAVE_MODEL_PREDICTIONS = True`, the scored test set is also persisted to
`workspace.bde.model_a_test_predictions` for faster downstream diagnostics and
dashboarding.

## 10. V4 Segment Findings

Model A is directionally useful, but the segment diagnostics show two important
patterns.

First, the model systematically underpredicts fares. On the October-December
2024 test split, average prediction bias is about -9 dollars for both taxi
colors:

| Segment | Trips | Average Actual | Average Predicted | Bias | MAE | RMSE |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Green | 150,850 | 24.21 | 14.96 | -9.25 | 9.36 | 13.02 |
| Yellow | 10,678,396 | 29.00 | 19.62 | -9.38 | 9.86 | 103.56 |

Second, RMSE is dominated by rare extreme fares. MAE is stable near 10 dollars
for the full test window, but RMSE spikes in November and in the 10-20 minute
duration bin:

| Segment | Trips | Bias | MAE | RMSE |
| --- | ---: | ---: | ---: | ---: |
| October | 3,739,487 | -9.33 | 9.82 | 13.36 |
| November | 3,545,286 | -9.14 | 9.62 | 178.70 |
| December | 3,544,473 | -9.66 | 10.11 | 13.69 |
| 10-20 Mins | 3,955,860 | -7.93 | 8.17 | 168.96 |

Route-pair diagnostics show where the model underpredicts systematically:

| Route Pair | Trips | Bias | MAE | RMSE |
| --- | ---: | ---: | ---: | ---: |
| Queens -> Unknown | 28,780 | -52.74 | 53.74 | 71.73 |
| Manhattan -> EWR | 28,371 | -52.31 | 52.43 | 55.15 |
| Queens -> Manhattan | 568,094 | -26.79 | 27.34 | 29.56 |
| Manhattan -> Queens | 293,992 | -20.21 | 21.06 | 24.13 |
| Manhattan -> Manhattan | 8,997,190 | -7.63 | 7.84 | 112.23 |

The airport and unknown-location routes likely need explicit features. The
Manhattan-to-Manhattan RMSE spike is not matched by its MAE, so it is likely
driven by rare extreme fares inside a very large segment rather than broad
day-to-day model failure.

## 11. Next Modeling Improvements

- Track model runs with MLflow.
- Promote Model E route calibration into the main reporting flow.
- Add explicit airport/EWR and unknown-location route features for remaining
  high-fare special cases.
- Train a true-label ridge model alongside the robust-label ridge model and
  compare RMSE, MAE, and bias.
- Add calendar features such as holiday and airport-period indicators.
- Compare against Spark MLlib linear regression and gradient-boosted trees on a
  sampled or feature-limited training set.
- Keep reporting full raw, capped, and operational-filtered metrics together so
  the outlier impact remains transparent.
