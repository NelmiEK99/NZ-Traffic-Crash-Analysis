
# NZ-Traffic-Crash-Analysis (2000–Mar 2025)

Analysis of New Zealand road crash data from the **Crash Analysis System (CAS)**.  
This repo contains a single notebook that explores crash patterns and severity drivers across **holidays, weather/light conditions, speed limits, regions/TLAs, and vehicle types**, and (optionally) builds **yearly ARIMA/SARIMA forecasts**.

## Notebook
- `NZ_Traffic_Crash_Analysis.ipynb`

## Key questions explored
1. Do crashes increase during holidays or under certain weather/light conditions?
2. Does speed limit impact crash severity?
3. Are certain regions (and TLAs) more prone to severe crashes?
4. Are certain vehicle types more frequently involved in crashes over time?

## Methods used
- Exploratory data analysis (counts, trends, distributions)
- **Chi-square tests** for association between categorical factors and crash severity
- Effect-size comparison (Cramér’s V / association strength)
- Visualisations (stacked bars, time trends, pie charts)
- **Power BI-ready exports** (CSV tables)
- Optional: **hierarchical ARIMA/SARIMA** workflow for yearly forecasting (total → severity → region)

## Dataset
The notebook expects a CAS crash dataset CSV (example file name used in the notebook):
- `Crash_Analysis_System_(CAS)_data.csv`

### Minimum columns used (subset)
- `crashYear` (or `crashFinancialYear`)
- `crashSeverity` (Fatal / Serious / Minor / Non-Injury)
- `holiday` / `holiday_filled`
- `weatherA`, `weatherB`
- `light`, `streetLight`
- `speedLimit` (used to create `speed_bin`)
- `region`
- `tlaId`, `tlaName` (optional deep dive)
- vehicle counts such as `carStationWagon`, `vanOrUtility`, `truck` (if present)

> ⚠️ Don’t commit the raw dataset to GitHub if it’s large or has licensing constraints.  
> Put it in a local `data/` folder and add `data/` to `.gitignore`.

## How to run
### 1) Create an environment (recommended)
```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -U pip
```

### 2) Install dependencies
```bash
pip install -U numpy pandas matplotlib scipy statsmodels pillow jupyter
```

### 3) Set the dataset path in the notebook
At the top of the notebook, update:

```python
CSV_PATH = "data/Crash_Analysis_System_(CAS)_data.csv"
```

(Optional) If you don’t have the banner image, you can ignore/remove `IMG_PATH`.

### 4) Run the notebook
Open the notebook in Jupyter / VS Code / Colab and run cells top-to-bottom.

## Outputs (Power BI exports)
The notebook writes several CSVs to:

- `./exports/region_by_severity.csv`
- `./exports/speedbin_by_severity.csv`
- `./exports/holiday_by_severity.csv`
- `./exports/vehicle_trends_by_year.csv`

These tables are designed to be imported directly into **Power BI**.

## Optional: Forecasting section (Yearly ARIMA/SARIMA)
The later part of the notebook includes a **3-step hierarchical workflow**:
1. Forecast total yearly crashes
2. Forecast yearly crashes by severity and reconcile to the total
3. Forecast yearly crashes by region and reconcile to the total

You can tweak:
```python
HORIZON_H = 5          # forecast horizon in years
TEST_START = None      # set a year boundary for backtesting
HOLIDAY_COL = "holiday_filled"  # set None if not available
```

## Suggested repo structure
```text
.
├── NZ_Traffic_Crash_Analysis.ipynb
├── README.md
├── exports/                 # generated outputs (optional to commit)
└── data/                    # keep local; add to .gitignore
```

## License
Add a license if you plan to share this publicly (e.g., MIT).
```
