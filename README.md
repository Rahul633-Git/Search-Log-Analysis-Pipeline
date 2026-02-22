# 🔍 Search Log Analysis Pipeline - Recommendation Based 

> **End-to-end ETL pipeline using PySpark, Databricks, and Delta Lake (Medallion Architecture) to identify high-demand regions for business expansion based on user search error logs.**

---

## 📌 Project Background

This project is built around a real-world business problem at a transfer/rides company:

When users search for a pickup or dropoff location where the company is **not yet operational**, those searches are logged as errors. Instead of discarding these logs, this pipeline analyzes them to **identify high-demand regions** the business should consider expanding into — turning failed searches into strategic expansion intelligence.

---

## 🎯 Business Objective

- Identify cities/regions with high search demand but no current service coverage
- Prioritize expansion targets based on search volume
- Focus on short-haul transfer markets (distance < 60 km) near airports
- Deliver insights via a Power BI map dashboard for leadership decision-making

---

## 🗂️ Dataset

Data sourced from company MySQL database (search error logs).

| Field | Description |
|-------|-------------|
| `pickup_location_name` | Name of the pickup location |
| `pickup_lat` | Latitude of pickup |
| `pickup_long` | Longitude of pickup |
| `destination_name` | Name of the destination |
| `destination_lat` | Latitude of destination |
| `destination_long` | Longitude of destination |
| `distance_km` | Distance between pickup and destination |
| `timezone` | Timezone of the search |

> **Note:** Data is anonymized and used with appropriate permissions. A sample synthetic dataset is provided for demonstration purposes.

---

## 🏗️ Architecture — Medallion (Bronze → Silver → Gold)
```
MySQL Database (Raw Search Error Logs)
        │
        ▼
   CSV Export
        │
        ▼
┌─────────────────────────────────┐
│         BRONZE LAYER            │
│  Raw ingestion into Delta Lake  │
│  No transformations applied     │
└─────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────┐
│         SILVER LAYER            │
│  - Drop nulls & fix data types  │
│  - Filter: distance_km < 60     │
│  - Flag: airport in location    │
│  - Exclude live regions         │
└─────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────┐
│          GOLD LAYER             │
│  - Aggregate by city/region     │
│  - Count searches per region    │
│  - Rank by search volume        │
│  - Output expansion suggestions │
└─────────────────────────────────┘
        │
        ▼
  Power BI Map Dashboard
  (Suggested expansion regions
   ranked by search demand)
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **PySpark** | Data cleaning, filtering, transformation |
| **Databricks** | Pipeline orchestration & notebook environment |
| **Delta Lake** | Bronze / Silver / Gold storage layers |
| **MySQL** | Source database (raw search logs) |
| **SQL** | Aggregation & analytical queries |
| **Python** | Supporting scripts & logic |
| **Power BI** | Final map dashboard visualization |

---

## 🔄 Pipeline Logic

### Bronze Layer
- Ingest raw CSV export from MySQL into Databricks
- Store as-is in Delta table — no transformations
- Preserves original data for auditability

### Silver Layer
- Remove null values and standardize data types
- **Filter:** `distance_km < 60` — focus on short-haul transfers
- **Airport detection:** flag rows where `pickup_location_name` OR `destination_name` contains "airport" (case-insensitive)
- **Exclusion:** filter out regions where the company is already operational

### Gold Layer
- Group by city/region
- Count total search logs per region
- Rank regions by search volume (descending)
- Output: prioritized list of suggested expansion cities

---

## 📊 Output — Power BI Dashboard

The final dashboard includes:
- 🗺️ **Map visual** — suggested cities plotted across India
- 📊 **Bar chart** — top regions ranked by search volume
- 🔍 **Filters** — by region, distance range, airport proximity

---

## 📁 Repository Structure
```
search-log-pipeline/
│
├── data/
│   └── sample_search_logs.csv
│
├── notebooks/
│   ├── 01_bronze_ingestion.ipynb
│   ├── 02_silver_transformation.ipynb
│   └── 03_gold_aggregation.ipynb
│
├── scripts/
│   ├── bronze_layer.py
│   ├── silver_layer.py
│   └── gold_layer.py
│
├── dashboard/
│   └── expansion_dashboard.pbix
│
└── README.md
```

---
<!-- 
## 🚧 Project Status

| Layer | Status |
|-------|--------|
| Bronze — Raw Ingestion | 🔄 In Progress |
| Silver — Cleaning & Filtering | 🔄 In Progress |
| Gold — Aggregation & Ranking | 📅 Planned |
| Power BI Dashboard | 📅 Planned |
| MySQL JDBC Integration (10M+ records) | 📅 Planned |

---
<
## 🔮 Future Enhancements

- Connect directly to MySQL via **JDBC connector** for 10M+ record production pipeline
- Add **Airflow** for pipeline orchestration and scheduling
- Integrate **Azure Data Lake Storage Gen2** for cloud storage layer
- Add **data quality checks** at each medallion layer
- Automate dashboard refresh via Power BI Service

---
-->
