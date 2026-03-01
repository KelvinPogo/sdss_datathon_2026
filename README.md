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

## Repository Structure
```
sdss_datathon_2026/
├── SDSS_datathon.ipynb          # Visualization
├── My_copy_SDSS_datathon.ipynb  # Model Training
├── requirements.txt
└── README.md
```

## Getting Started

### Prerequisites
- Python 3.12+
- Jupyter Notebook or JupyterLab

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/KelvinPogo/sdss_datathon_2026.git
   cd sdss_datathon_2026
   ```

2. Install required dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Launch Jupyter:
   ```bash
   jupyter notebook
   ```

4. Open `SDSS_datathon.ipynb` to view the main analysis.

## Usage
- **`SDSS_datathon.ipynb`** — Scroll to the very end of the notebook and check out the interactive map.
- **`My_copy_SDSS_datathon.ipynb`** — Model analysis was done here.

## Authors
**KelvinPogo** — [GitHub Profile](https://github.com/KelvinPogo)  
**shrEkt101** — [GitHub Profile](https://github.com/shrEkt101)

## License
This project is open source. See the repository for details.
