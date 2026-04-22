HW1 — Aircraft Inventory Data Engineering
Name : Rhema Phiri

This file performs end-to-end data engineering on the BTS Aircraft Inventory dataset (`T_F41SCHEDULE_B43`), covering missingness investigation, imputation, standardization, transformation, feature engineering, and predictive modeling.

## How to Run
- Run all cells from top to bottom as each section depends on actions taken in previous steps. Each function is numbered from 1 to 11 and should be run in that order. The input to functions is primarily the dataset, and in one instance, the column name. The functions mainly perform systemic changes and the only output is the changed dataset.


## Expected Outputs
Section 1 — Missingness:
- `missingno` bar chart showing missing data across all columns
- Tables showing remaining missing rows after imputation for `CARRIER`, `CARRIER_NAME`, `AIRLINE_ID`
- RMSE comparison table for Mean, Median, KNN, and PMM imputation methods for both `NUMBER_OF_SEATS` and `CAPACITY_IN_POUNDS`

Section 2 — Standardization:
- Value counts for `OPERATING_STATUS` (should show only `Y`, `N`, `NaN`)
- Value counts for `AIRCRAFT_STATUS` (should show only `A`, `B`, `L`, `O`)
- Value counts for `MANUFACTURER` and `MODEL` after mapping

Section 3 — Dropping Missing:
- Row count before and after dropping
  
Section 4 — Transformation:
- Skewness values printed for both columns before transformation
- Two histograms before transformation
- Two histograms after Box-Cox transformation showing reduced skewness

Section 5 — Feature Engineering:
- Value counts for `SIZE` column (SMALL, MEDIUM, LARGE, XLARGE)
- Two side-by-side bar plots: Operating Status by Size and Aircraft Status by Size

Section 6 — Modeling:
- RMSE results table
