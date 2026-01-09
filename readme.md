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

## Modeling & AutoML Replication Scripts

In addition to AutoML workflows, I designed standalone Python notebooks that replicate the final selected AutoML models. These scripts allow stakeholders to reproduce predictions even when AutoML tools are unavailable, ensuring transparency, portability, and long-term usability.

### 4. `04_Model_TimeSeries.ipynb`

This notebook replicates the AutoML **time-series forecasting** model used to predict future traffic volumes.

**Key Capabilities**
- Trains time-series models on engineered daily and monthly traffic features  
- Incorporates seasonality, trends, and lagged features  
- Produces forward-looking traffic volume forecasts by facility  
- Designed to mirror AutoML-selected features and evaluation logic  

**Use Case**
- Forecast future congestion patterns  
- Support planning, staffing, and seasonal capacity decisions without AutoML dependency  

---

### 5. `05_Model_TollViolation.ipynb`

This notebook reproduces the **toll violation classification** model originally selected via AutoML.

**Key Capabilities**
- Trains supervised classification models on the balanced violation dataset  
- Uses the same feature set generated during feature engineering  
- Handles class imbalance explicitly to avoid majority-class bias  
- Outputs violation probability scores and class predictions  

**Use Case**
- Predict likelihood of toll violations by facility and time  
- Support enforcement planning and compliance strategies  
- Serve as a drop-in replacement for AutoML-based classifiers  

---

### 6. `06_Model_TrafficCluster.ipynb`

This notebook replicates the **traffic volume category prediction** model based on facility-level clustering.

**Key Capabilities**
- Trains classification models to predict **High / Medium / Low** traffic volume levels  
- Uses clustered labels generated during facility-level aggregation  
- Enables prediction of traffic intensity instead of raw counts  
- Aligns with AutoML-selected clustering and classification logic  

**Use Case**
- Classify future days into traffic intensity levels  
- Support operational decision-making and congestion alerts  
- Maintain consistency with AutoML clustering outputs without platform reliance  

---

### Why These Scripts Matter

Together, these three notebooks ensure that:
- All final AutoML results are **fully reproducible** in pure Python  
- Stakeholders can run, audit, and extend models without vendor lock-in  
- The project remains deployable in environments where AutoML tools are unavailable  

These scripts complete the end-to-end pipeline from raw data → feature engineering → modeling → prediction → visualization.

---

## Data Visualization (Tableau Dashboards)

### Tableau Public Dashboard
I built fully interactive dashboards in Tableau to answer the four core project questions (Q1–Q4) related to traffic usage, toll violations, congestion patterns, and pricing impact across Port Authority bridges and tunnels.

The dashboards allow users to filter by year, facility, vehicle type, and other dimensions, and explore trends without additional data preparation.

🔗 **View the interactive Tableau dashboard on Tableau Public:**  
https://public.tableau.com/views/PortAuthorityofNYNJ/Q1-TopFactorsAffectingBridgeTunnelUsage?:language=en-GB&:display_count=n&:origin=viz_share_link

---

### Medium Articles – End-to-End Project Explanation
I documented the full project journey in a four-part Medium series, explaining the analysis, modeling, deployment approach, and final insights step by step. These articles provide detailed context behind the datasets, feature engineering, models, and dashboards included in this repository.

**Part 1 – Data Exploration & Problem Framing**  
https://medium.com/@ssrivastava09/exploring-traffic-data-for-the-port-authority-of-ny-nj-part-1-70089db7c56c

**Part 2 – Building the Models**  
https://medium.com/@ssrivastava09/exploring-traffic-data-for-the-port-authority-of-ny-nj-part-2-building-the-models-7787d6bf2b1f

**Part 3 – The Deployment Journey**  
https://medium.com/@ssrivastava09/exploring-traffic-data-for-the-port-authority-of-ny-nj-part-3-the-deployment-journey-c6e571e9fe1e

**Part 4 – From Analysis to Final Insights**  
https://medium.com/@ssrivastava09/exploring-traffic-data-for-the-port-authority-of-ny-nj-part-4-final-part-from-analysis-to-8cc62e4c641a

Together, the Tableau dashboards and Medium articles provide both an interactive and narrative view of the project—from raw data and feature engineering to modeling, deployment, and actionable recommendations.

---

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

## Model Deployment & API Inference ##
All three selected models were deployed after evaluation using **Azure AutoML**. The deployment was carried out using an **Azure AutoML Python Notebook running inside a virtual environment**, which provided greater flexibility and control compared to Azure App Services. 

**Deployment Approach**

• Models were trained and finalized using Azure AutoML instances

• Deployment executed via Azure AutoML Python Notebook (Virtual Environment)

• This approach allowed custom preprocessing, version control, and reproducibility

**API Exposure**

Once deployed, the models were exposed as REST APIs using **ngrok**, which securely exposed the local service to external users through a public URL.
After the API was exposed:

• External clients could send JSON-based HTTP requests

• The API returned real-time predictions in JSON format

This setup ensures that even in the absence of AutoML infrastructure, stakeholders can replicate predictions using the provided Python scripts.

**API Request & Response Example**

Below is an example **Postman JSON request** used for predicting monthly traffic volume, followed by the corresponding API response.

**Sample Request (Postman – Predict Monthly Traffic Volume)**

<img width="480" height="416" alt="image" src="https://github.com/user-attachments/assets/b4c551d1-9618-4b0d-8238-5c007cf1a610" />


**Sample Response**

<img width="480" height="415" alt="image" src="https://github.com/user-attachments/assets/0827d0d0-1ffc-4dd6-8dcf-7e7e8f4326a1" />

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
