# Assignment Brief

## 1. Aim

Analyse a large NYC taxi dataset in Databricks Spark. The work must load and
prepare the data, answer business questions with Spark SQL, and train machine
learning models to predict a continuous outcome: `total_amount`.

## 2. Dataset Context

The dataset comes from the New York City Taxi and Limousine Commission. It
contains yellow and green taxi trip records with pickup/drop-off timestamps,
locations, trip distance, fare components, rate type, payment type, and
passenger count.

- Yellow taxis can pick up street-hail passengers across NYC.
- Green taxis were introduced in August 2013 to improve service availability
  in outer boroughs and designated areas.

## 3. Required Work

### 3.1 Data Ingestion and Preparation

- Load green and yellow taxi trip data in Databricks.
- Import the taxi zone lookup CSV.
- Explore schemas and clean unrealistic trips.
- Do not remove rows solely because unused fields are missing.
- Do not remove more than 10% of the dataset.
- Count total rows for green and yellow taxis.
- Combine green and yellow trips despite schema differences.
- Join pickup and drop-off locations to borough metadata.
- Save the final cleaned and joined dataset as a table.
- Count final table rows.

### 3.2 Business Questions

Use SQL outputs for:

- monthly trip count, peak weekday, peak hour, average passengers, average
  paid per trip, and average paid per passenger;
- taxi-color duration, distance, and speed summaries;
- route demand and revenue by color, borough pair, month, weekday, and hour;
- 2024 top 10 borough-pair revenue share;
- tip participation and high-tip share;
- duration-bin speed and distance-per-dollar analysis;
- recommendation on which duration bin drivers should target for income.

### 3.3 Machine Learning

- Build at least two different models to predict `total_amount`.
- Use the Part 2 route-time average as a baseline and calculate RMSE.
- Train/validate on data excluding October-December 2024.
- Test on October, November, and December 2024.
- Compare models using RMSE and explain the chosen model.
- Do not use `fare_amount` or `tolls_amount` as model features.

## 4. Deliverables

- Notebook in both `.ipynb` and HTML format.
- Handover report as PDF, under 2500 words.
- Any supporting documents needed for review.
- Final submission as one zip file.

## 5. Assessment Focus

- Executable, well-commented code.
- Justified transformation, cleaning, storage, and accuracy decisions.
- Correct business-question outputs and useful recommendations.
- Relevant machine learning models and comparison.
- Clear, professional written report.
