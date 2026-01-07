# Port Authority of NY & NJ  
## Tunnels, Bridges & Traffic Analytics

This repository contains Jupyter notebooks and curated datasets that I developed to clean, merge, analyze, and engineer transportation data for the Port Authority of NY & NJ. The project covers Port Authority Bus Terminal (PABT) bus and passenger records as well as bridges and tunnels traffic data.  
The workflows are designed to support exploratory analysis, predictive modeling, AutoML pipelines, and Power BI and Tableau reporting.

---

## Notebooks Overview

### 1. `data_ingestion_bus_passenger.ipynb`

This notebook merges PABT Bus and Passenger datasets using **Carrier**, **StartDate**, and **EndDate** as join keys.

**Key Capabilities**
- Aggressive carrier name standardization (case, whitespace, punctuation) to improve merge accuracy  
- Data type normalization, including day-first date parsing and passenger date decomposition  
- Comprehensive outer join with zero-filling for missing carrier–date combinations  
- Optional modeling enhancements:
  - Log-transformed volumes  
  - Outlier capping  
  - Carrier-level normalization  
  - Zero-inflation flags and zero-inflated Poisson features  

**Outputs**
- Full raw merged dataset (for auditing)
- Reduced analytical dataset with core fields:  
  `StartDate, EndDate, CarrierClean, VolumeBus, VolumePassenger`

---

### 2. `exploratory_data_analysis.ipynb`

A reusable, dataset-agnostic EDA framework that I apply across all major datasets.

**Key Capabilities**
- Automated checks for nulls, duplicates, whitespace issues, zero-length strings, and high cardinality  
- Data validation and summary statistics  
- Auto-generated visualizations (histograms, boxplots, scatterplots, correlations)  
- Modular design — requires only a DataFrame and dataset name  
- Progress tracking for large datasets  

---

### 3. `feature_engineering_traffic.ipynb`

This notebook prepares raw traffic data for classification, clustering, AutoML, and BI reporting, with a focus on scalability and modeling readiness.

**Key Capabilities**
- Chunk-based ingestion for large traffic datasets  
- Standardization of date/time fields and categorical encoding  
- Robust data-quality checks and class-total validation  
- Advanced feature engineering:
  - Temporal features: `IsWeekend`, `IsHoliday`, `Season`, `WeekOfYear`, `Hour`, `TimeOfDay`  
  - Trend features: lagged values and rolling statistics  
  - Traffic metrics: vehicle-class totals, violations, and derived rates  
- Creation of a balanced classification dataset to address class imbalance while preserving temporal structure  
- Facility-level aggregation and clustering to derive **High / Medium / Low** traffic volume categories  
- Generation of analytics-ready datasets for Power BI and AutoML

**Outputs**
- Fully engineered daily traffic dataset  
- Balanced classification dataset for AutoML  
- Facility-level clustered dataset with semantic traffic labels  
- Power BI–ready daily, weekly, and monthly aggregates  

---

## Data Visualization (Tableau Dashboards)

I built interactive Tableau dashboards using traffic, toll transaction, and violation data from **2013–2025** to answer four project questions. Each dashboard is labeled **Q1–Q4** and includes filters for year, facility, and vehicle type.



**Q1 – Factors Affecting Usage**  
Analyzes traffic volume by facility, weekday vs weekend, holiday vs non-holiday, vehicle mix, and toll payment method. Results show that weekday, non-holiday auto traffic drives most usage, with GWB Upper and Lower carrying the highest volumes.

<img width="1319" height="828" alt="Q1 - Top Factors Affecting Bridge   Tunnel Usage" src="https://github.com/user-attachments/assets/1ffa8a1f-2f1a-449d-951d-a60cafd788e0" />




**Q2 – Toll Violations by Time and Facility**  
Explores violation patterns by facility and season, showing strong concentration at high-volume crossings and peaks during summer months.
 
<img width="1282" height="831" alt="Q2 - Toll Violations by Facility and Month" src="https://github.com/user-attachments/assets/ae659322-025c-4fcd-90ec-72d413b8d00e" />




**Q3 – Busiest Times and Traffic Drivers**  
Identifies peak congestion periods using daily, weekly, and monthly views. The analysis shows congestion is highest during summer weekends, especially Saturdays, and is primarily driven by passenger cars.

<img width="1286" height="826" alt="Q3 – Busiest Times and Traffic Drivers" src="https://github.com/user-attachments/assets/1ca6a8de-50e8-440f-9acf-ce010680ac89" />




**Q4 – Impact of Pricing on Congestion**  
Compares 2025 traffic with prior years and shows stable facility shares and volumes, with no strong evidence of system-wide congestion reduction due to pricing changes.

<img width="1251" height="833" alt="Q4 - Has pricing reduced congestion in 2025?" src="https://github.com/user-attachments/assets/7759e0ca-c730-4446-b2e4-e6fd94b1d615" />


---

## Data Files

### `Traffic_PowerBI_Ready_Daily.csv`
**Description:**  
Daily-level traffic dataset for operational analysis and visualization.

**Key Fields:**  
`DATE, FAC, LANE, FAC_B, Is_Holiday, Is_Weekend, Day_Name`

---

### `Traffic_PowerBI_Ready_Monthly.csv`
**Description:**  
Monthly aggregated dataset designed for executive dashboards and long-term traffic trend analysis.

**Grouped By:**  
`month_year, FAC, LANE, FAC_B`

---

### `Traffic_Classification_Balanced.csv`
**Description:**  
Balanced, classification-ready dataset created to address severe class imbalance in traffic violation prediction tasks.

**Purpose**
- Enables reliable AutoML and supervised learning workflows  
- Preserves temporal structure  
- Reduces bias toward majority classes  

---

### `Traffic_Facility_Clustered_With_Cluster.csv`
**Description:**  
Facility-level daily traffic dataset enriched with clustering outputs.

**Purpose**
- Assigns **High / Medium / Low** traffic volume categories per facility  
- Retains cluster identifiers for validation and exploratory analysis  
- Supports facility-specific traffic pattern discovery  
