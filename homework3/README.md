# NYC Taxi Trip Analysis — Homework 3

Analysis of NYC TLC Yellow and Green Taxi trip records (January 2026) using PySpark on AWS EC2.

---

## You will need AWS credentials with access to the course S3 bucket and your own S3 bucket


## Running the Notebook

Run all cells **in order** from top to bottom.

| Section | What it does |
|---------|-------------|
| **Data Download** | Downloads Yellow and Green taxi parquet files from S3 into `data/` |
| **Part 1 – EC2 & Spark Setup** | Installs dependencies and initializes a local Spark session |
| **Part 2 – Start Spark** | Creates the SparkSession |
| **Part 3 – Load Data** | Reads parquet files into Spark DataFrames |
| **Part 4 – Standardize & Combine** | Renames datetime columns, tags taxi type, unions datasets |
| **Part 5 – Clean Data** | Applies cleaning rules (invalid timestamps, distances, fares, durations) |
| **Part 6 – Analytics (Q1–Q7)** | Answers analytical questions using Spark DataFrame operations |
| **Part 7 – ML Model (Q8)** | Trains a Random Forest to predict fare amount; reports RMSE, MAE, R² |
| **Part 8 – Write to S3** | Saves result tables and plots to your S3 bucket |

---

## Output Files

After running the notebook, the following files will be saved locally under `outputs/` and uploaded to S3:

| File | Description |
|------|-------------|
| `outputs/actual_vs_predicted.png` | Scatter plot of predicted vs actual fare amounts |
| `outputs/feature_importances.png` | Bar chart of Random Forest feature importances |
| `outputs/trips_by_type/` | Parquet — trip counts by taxi type (Q1) |
| `outputs/avg_fare_by_type/` | Parquet — average fare by taxi type (Q2) |

---

## S3 Output Paths

Results are written to:

```
s3://phiri-rhema-lab3/outputs/
s3://phiri-rhema-lab3/nyc-taxi-assignment/
```
