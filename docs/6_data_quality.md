# Data Quality

This document summarizes the cleaning logic used to build the curated NYC taxi
trip table.

## 1. Quality Goal

The cleaning strategy removes trips that are physically impossible,
operationally unrealistic, or unsuitable for fare modeling, while preserving
valid trips with missing values in fields that are not required for analysis.

## 2. Row Retention

| Metric | Value |
| --- | ---: |
| Raw rows | 991,467,464 |
| Clean rows | 974,672,551 |
| Removed rows | 16,794,913 |
| Removed percentage | 1.69% |

The retained dataset is large enough for stable route analytics and model
training while removing obvious quality defects.

## 3. Filter Checklist

| Rule | Reason |
| --- | --- |
| Drop trips where drop-off is not after pickup | Removes impossible or corrupt timestamps |
| Keep duration from 1 to 180 minutes | Removes zero-length and extreme-duration trips |
| Keep distance from 0.1 to 200 km | Removes zero-distance trips and extreme outliers |
| Keep speed from 0 to 120 km/h | Removes negative and physically unrealistic speeds |
| Keep populated passenger counts from 0 to 6 | Allows nullable counts while removing extreme entries |
| Require non-negative `total_amount` | Protects the ML target from invalid fare totals |
| Validate populated pickup and drop-off zone IDs | Preserves join quality with the zone lookup |
| Enforce service start dates by taxi color | Aligns yellow and green trips with TLC service history |
| Validate populated rate/payment/trip flags | Removes invalid categorical codes without dropping nulls |

## 4. Modeling Implications

- The model target is `total_amount`, so negative total fares are excluded.
- `fare_amount` and `tolls_amount` are not used as model features because they
  would leak direct fare information into the prediction task.
- Extreme but valid fare values remain in the true-label evaluation, which is
  why test RMSE is much higher than robust capped-label diagnostics.
- Robust label clipping is fitted from training data only and used as a
  diagnostic lens, not as the final true-label metric.

## 5. Review Notes

The quality rules are intentionally explicit in the notebook Markdown and SQL
cells. This makes the cleaning assumptions auditable and keeps the curated table
usable for both business analytics and modeling.
