# SDSS Datathon 2026
A data science competition submission for the **2026 Symposium on Data Science & Statistics (SDSS) Datathon**.

## Overview
This repository contains Jupyter notebooks developed for the SDSS Datathon 2026. The project analyzes daily operational data from **Toronto's shelter and overnight services system** across 2024–2025, with the goal of assessing system pressure, capacity utilization, and operational risk across programs over time.

## The Challenge
Toronto's shelter system supports thousands of people every night under complex, dynamic conditions — fluctuating demand, seasonal pressures, capacity constraints, and differing service models across populations. When occupancy approaches capacity, the system becomes less resilient to shocks and harder to manage operationally.

This project treats the dataset as a **panel of shelter programs observed daily** and applies data science techniques to:
- Identify service sectors that consistently operate near or at full capacity
- Compare average vs. peak occupancy events to assess system resilience
- Assess how temporary capacity losses (unavailable beds/rooms) contribute to system strain
- Examine daily and seasonal trends in occupancy and capacity utilization
- Detect programs that are more operationally fragile through variability analysis

Findings are translated into **data-driven recommendations** for improving operational planning, capacity buffering, and service coordination within the shelter system.

## Dataset
The dataset is sourced from the City of Toronto's shelter management system and contains daily occupancy records.

### Original Variables

| Variable | Description | Type |
|---|---|---|
| `OCCUPANCY_DATE` | Date of the overnight occupancy record (YYYY-MM-DD) | Date |
| `LOCATION_POSTAL_CODE` | Postal code of the service location | Categorical |
| `SECTOR` | Population served (Adult Men, Adult Women, Mixed Adult, Youth, Family) | Categorical |
| `OVERNIGHT_SERVICE_TYPE` | Type of service (Shelter, Motel/Hotel, 24-Hour Respite, Warming Centre, etc.) | Categorical |
| `PROGRAM_MODEL` | Emergency or Transitional classification | Categorical |
| `PROGRAM_AREA` | Base system or temporary response program | Categorical |
| `CAPACITY_TYPE` | Whether capacity is measured in beds or rooms | Categorical |
| `ACTUAL_CAPACITY` | Beds or rooms available on the date | Continuous |
| `OCCUPIED_CAPACITY` | Beds or rooms occupied on the date | Continuous |
| `UNAVAILABLE_CAPACITY` | Beds or rooms temporarily out of service | Continuous |
| `OCCUPANCY_RATE` | Occupied capacity divided by actual capacity (0–1) | Continuous |

> Note: Full postal codes are included; address and name fields have been removed. Capacity figures are standardized across beds and rooms. Occupancy rate is the primary system pressure indicator.

### Engineered Features

| Variable | Description | Type |
|---|---|---|
| `LATITUDE` | Latitude coordinate of the service location, geocoded from postal code | Continuous |
| `LONGITUDE` | Longitude coordinate of the service location, geocoded from postal code | Continuous |
| `DAY_NUM` | Day number relative to the start of the dataset (1 = Jan 1, 2024) | Continuous |
| `MONTH` | Month extracted from `OCCUPANCY_DATE` | Continuous |
| `DAY_OF_WEEK` | Day of week extracted from `OCCUPANCY_DATE` (0 = Monday) | Continuous |
| `CRITICAL_95` | Binary label: 1 if `OCCUPANCY_RATE` ≥ 0.95, else 0 (model target) | Binary |
| `FULL_100` | Binary flag: 1 if `OCCUPANCY_RATE` = 1.0 (fully at capacity) | Binary |

## Data Cleaning
The raw dataset contained **100,337 rows**. The following cleaning steps were applied:

1. **4 rows** with a missing `PROGRAM_MODEL` value (a "Top Bunk Contingency Space" Youth program in January 2025) were identified and dropped.
2. A further **4,512 rows** with a missing `LOCATION_POSTAL_CODE` were removed.
3. The final clean dataset used for modeling contains **95,821 rows** and is saved as `clean_data.csv`.

## Modeling

### Target Variable
The model predicts `CRITICAL_95` — a binary flag indicating whether a program's daily occupancy rate reached or exceeded **95%**, the threshold used as the primary system pressure indicator.

Notably, the dataset is heavily imbalanced: **88.3%** of days are at or above the critical threshold, and **78.7%** are at full capacity (100%).

### Features Used
`ACTUAL_CAPACITY`, `UNAVAILABLE_CAPACITY`, `MONTH`, `DAY_OF_WEEK`, `SECTOR`, `OVERNIGHT_SERVICE_TYPE`, `PROGRAM_MODEL` (categorical features one-hot encoded).

### Models Trained

An 80/20 train-test split was used with `random_state=42`.

**Logistic Regression (default)**
| Metric | Non-Critical (0) | Critical (1) |
|---|---|---|
| Precision | 0.53 | 0.88 |
| Recall | 0.03 | 1.00 |
| F1-Score | 0.06 | 0.94 |
| ROC-AUC | — | **0.808** |

**Logistic Regression (class_weight="balanced")**
| Metric | Non-Critical (0) | Critical (1) |
|---|---|---|
| Precision | 0.32 | 0.96 |
| Recall | 0.78 | 0.78 |
| F1-Score | 0.46 | 0.86 |
| ROC-AUC | — | **0.807** |

**XGBoost (scale_pos_weight adjusted for class imbalance)** ✅ Best model
| Metric | Non-Critical (0) | Critical (1) |
|---|---|---|
| Precision | 0.53 | 0.99 |
| Recall | 0.93 | 0.89 |
| F1-Score | 0.67 | 0.93 |
| ROC-AUC | — | **0.965** |

XGBoost significantly outperformed both logistic regression variants, achieving a ROC-AUC of **0.965** and strong recall on the minority class (non-critical days), making it the most operationally useful model for early identification of lower-pressure periods.

## Repository Structure
```
sdss_datathon_2026/
├── SDSS_datathon.ipynb          # Visualization & interactive map
├── My_copy_SDSS_datathon.ipynb  # Data cleaning & model training
├── clean_data.csv               # Cleaned dataset (95,821 rows)
├── requirements.txt
└── README.md
```

## Running the Notebooks

Both notebooks are designed to run in **Google Colab**.

### Step 1 — Download the cleaned dataset

Download `clean_data.csv` from this repository:
```
https://github.com/KelvinPogo/sdss_datathon_2026/raw/main/clean_data.csv
```

### Step 2 — Open the notebook in Colab

Click the **Open in Colab** badge at the top of either notebook, or open it manually at [colab.research.google.com](https://colab.research.google.com) via `File → Open notebook → GitHub`.

### Step 3 — Upload the CSV to Colab

Drag and drop `clean_data.csv` into the 📁 **Files** panel on the left sidebar in Colab.

### Step 4 — Run all cells

Go to `Runtime → Run all` to execute the full notebook from top to bottom.

## Authors
**KelvinPogo** — [GitHub Profile](https://github.com/KelvinPogo)  
**shrEkt101** — [GitHub Profile](https://github.com/shrEkt101)

## License
This project is open source. See the repository for details.
