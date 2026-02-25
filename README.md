# GDP Nowcasting with Dynamic Factor Model (DFM)
This project builds a simple mixed-frequency GDP nowcasting pipeline using a **Dynamic Factor Model (DFM)** in Python (`statsmodels`).
## Project Goal
Use monthly macroeconomic indicators and quarterly real GDP to estimate a DFM and produce GDP nowcasts.
## Data
Macro series are from **FRED** (monthly indicators + quarterly GDP), from 1990/01/01 to the latest including:
- GDPC1 (Real GDP, transformed)
- INDPRO
- PAYEMS
- HOUST
- RSXFS
- CPIAUCSL
- UNRATE
- FEDFUNDS
- GS10
- BAA10YM
- T10Y2Y
## Target Variable
In the modeling dataset, the GDP column is transformed into:
- **Quarterly annualized log growth**
- Formula: `400 * Δlog(GDP)`
The quarterly GDP growth series is aligned to **quarter-end months** on a monthly-start index (`MS`), e.g.:
- `2025Q4 -> 2025-12-01`
- `2026Q1 -> 2026-03-01`
## Model
Main specification used in this notebook:
- **DFM(2,2)**  
  - `n_factors = 2`
  - `factor_order = 2`
## Project Summary (What I Did, What I Found, and Results)
### What I did
- Built a mixed-frequency GDP nowcasting pipeline in Python using a **Dynamic Factor Model (DFM)**.
- Combined quarterly real GDP with monthly macroeconomic indicators from FRED.
- Transformed GDP into **quarterly annualized log growth** (`400 * Δlog(GDP)`), and aligned quarterly values to quarter-end months on a monthly-start index.
- Preprocessed monthly indicators (including log-difference transformations for selected real activity and price variables).
- Estimated and compared several DFM specifications, then selected **DFM(2,2)** as the main model for nowcasting.
- Generated **smoothed latent factors** and visualized them for in-sample interpretation.
- Produced nowcasts for **2025Q4** and **2026Q1**.
### What I found
- A DFM with **2 factors and factor order 2** provides a stable and interpretable specification for this dataset.
- The smoothed factors capture broad co-movements across real activity, labor, prices, and financial indicators.
- The mixed-frequency setup allows quarterly GDP growth to be estimated using monthly information in a unified monthly timeline.
### Results
- **2025Q4 GDP nowcast (QoQ annualized): 2.964%**
- **2026Q1 GDP nowcast (QoQ annualized): 3.034%**
These results are generated using information available before each target month label in the mixed-frequency dataset.

# Reproduction Guidelines
## How to Run
1. Open `notebooks/DFM_Memo.ipynb`
2. Make sure required packages are installed (`pandas`, `numpy`, `matplotlib`, `statsmodels`)
3. Update file paths if needed (prefer relative paths)
4. Run cells in order
5. If raw CSV files are not included, use the processed dataset `data/DFM_data.csv`
## Notes
- This version focuses on the DFM nowcasting workflow and result generation.
- Rolling out-of-sample evaluation vs benchmarks (e.g., AR(1)) can be added as a future extension.
