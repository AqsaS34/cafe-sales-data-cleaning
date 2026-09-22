# cafe-sales-data-cleaning
Cleaned a messy 10K-row cafe sales dataset ,handled disguised missing values, fixed data types, and imputed missing prices using column relationships.


## About the Dataset

The raw dataset (`dirty_cafe_sales.csv`) contains 10,000 simulated cafe transactions with 8 columns: Transaction ID, Item, Quantity, Price Per Unit, Total Spent, Payment Method, Location, and Transaction Date.

## Problems Found in the Raw Data

- **Disguised missing values** — many cells contained the literal text `"ERROR"` or `"UNKNOWN"` instead of being truly blank. Because these looked like valid text, pandas did not recognize them as missing data by default, and they were hiding roughly 600+ additional missing values that a simple `.isnull()` check would have missed.
- **Incorrect data types** — `Quantity`, `Price Per Unit`, and `Total Spent` were stored as text instead of numbers, caused by the "ERROR"/"UNKNOWN" values mixed into otherwise numeric columns.
- **Genuine missing values** — separate from the disguised ones, several columns had real blank cells (e.g. `Item`, `Payment Method`, `Location`, `Transaction Date`).
- **No duplicate rows** — checked using `.duplicated()`, none were found.

## Cleaning Steps

1. Replaced all `"ERROR"` and `"UNKNOWN"` placeholder text with proper `NaN` values so they could be correctly detected as missing.
2. Converted `Quantity`, `Price Per Unit`, and `Total Spent` from text to numeric types using `pd.to_numeric()`.
3. Filled missing numeric values (`Quantity`, `Price Per Unit`, `Total Spent`) using the column mean.
4. Filled missing text values (`Item`, `Payment Method`, `Location`) with the label `"Unknown"`.
5. Removed rows with a missing `Transaction Date`, since a missing date could not be reasonably recovered.
6. Converted `Transaction Date` from text into a proper datetime type.
7. Checked for and confirmed there were no duplicate rows.
8. Verified the final result using `.info()` and `.describe()` to confirm correct data types and sensible value ranges (no negative numbers, no extreme outliers).

## Result

- **Rows before cleaning:** 10,000
- **Rows after cleaning:** 9,540 (rows with missing dates removed)
- **Columns:** 8
- All columns now have complete data and correct types (numeric columns as `float64`, date column as `datetime64`).

## Files in This Repository

| File | Description |
|---|---|
| `dirty_cafe_sales.csv` | Original raw dataset, before cleaning |
| `cafe_sales_cleaning.ipynb` | Jupyter Notebook with the full step-by-step cleaning process |
| `cafe_sales_cleaned.csv` | Final cleaned dataset |
| `README.md` | This file |

## Tools Used

Python, pandas, Jupyter Notebook
