# 📊 Search Log Analysis & Expansion Intelligence Pipeline

[![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)](https://databricks.com/)
[![Apache Spark](https://img.shields.io/badge/Apache_Spark-E25A1C?style=for-the-badge&logo=apache-spark&logoColor=white)](https://spark.apache.org/)
[![Delta Lake](https://img.shields.io/badge/Delta_Lake-00ADD8?style=for-the-badge&logo=databricks&logoColor=white)](https://delta.io/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)

> **Production-grade ETL pipeline implementing Medallion Architecture to identify high-demand expansion regions from ride-search error logs**

---

## 🚀 Overview

This project demonstrates an **end-to-end data engineering solution** built on **Databricks** using **PySpark** and **Delta Lake**. It implements the industry-standard **Medallion Architecture** (Bronze → Silver → Gold) to process ride-search logs and generate actionable expansion intelligence metrics.

The pipeline identifies under-served markets with high demand, enabling data-driven expansion decisions for ride-hailing platforms.

### Key Highlights
- ✅ **Production-oriented design** with data quality enforcement
- ✅ **Automated orchestration** via Databricks Workflows
- ✅ **Version-controlled** with Git integration
- ✅ **Scheduled daily execution** for batch processing
- ✅ **Audit-enabled** Bronze layer for traceability
- ✅ **Business-focused** Gold layer metrics

---

## 🏗️ Architecture

The pipeline follows the **Medallion Architecture** pattern with **flexible data source connectivity**:

```
┌────────────────────────────────────────────┐
│         Data Source Options                │
│  ┌───────────┐  ┌───────┐  ┌────────────┐  │
│  │ Mock Data │or│ MySQL │or│ PostgreSQL │  │
│  │ Generator │  │   DB  │  │     DB     │  │
│  └─────┬─────┘  └────┬──┘  └────┬───────┘  │
└────────┼─────────────┼──────────┼──────────┘
         │             │          │
         └─────────────┴──────────┘
                       │
                       ▼
         ┌─────────────────────────┐
         │     Bronze Layer        │ ◄── Raw Data Landing (Audit-Enabled)
         │     (Delta Table)       │     • Schema enforcement
         └───────────┬─────────────┘     • Ingestion metadata
                     │                   • Partitioned storage
                     ▼
         ┌─────────────────────────┐
         │     Silver Layer        │ ◄── Data Quality & Transformation
         │     (Delta Table)       │     • Type casting
         └───────────┬─────────────┘     • Data validation
                     │                   • Outlier removal
                     ▼
         ┌─────────────────────────┐
         │      Gold Layer         │ ◄── Business Intelligence
         │     (Delta Table)       │     • Aggregated metrics
         └───────────┬─────────────┘     • Expansion signals
                     │                   • City ranking
                     ▼
         ┌─────────────────────────┐
         │   Analytics/Dashboard   │
         └─────────────────────────┘
```

### 🔌 Data Source Flexibility

This pipeline supports **multiple ingestion patterns**:

| Source | Use Case | Implementation |
|--------|----------|----------------|
| **Mock Data Generator** | Development, Testing, Demos | Python synthetic data generation |
| **MySQL Database** | Production OLTP source | JDBC connector with incremental load |
| **PostgreSQL Database** | Production OLTP source | JDBC connector with incremental load |
| **Cloud Storage** | File-based ingestion | S3, ADLS, GCS support |

---

## 📁 Project Structure

```
Search-Log-Analysis-Pipeline/
│
├── notebooks/
│   ├── 00_data_ingestion/
│   │   └── mock_data_generator.ipynb  or mysql/postgres_connector   # incase of direct db connection 
│   │                                                                # here only workring with ~100k mock data 
│   │   
│   │
│   ├── 01_bronze_layer.ipynb             # Raw data landing
│   ├── 02_silver_layer.ipynb             # Data cleaning & validation
│   └── 03_gold_layer.ipynb               # Business metrics computation
│
├── config/
│   ├── pipeline_config.py                # Centralized configuration
│   ├── db_connections.py                 # Database connection configs
│   └── schemas.py                        # Schema definitions
│
├── scripts/
│   └── setup_source_db.sql               # Source database setup (MySQL/PostgreSQL)
│
└── README.md                              # Project documentation

---





## ⚙️ Orchestration & Automation

### Databricks Workflow

The pipeline is orchestrated using **Databricks Workflows** with the following DAG:

```
generate_mock_data
        ↓
bronze_layer
        ↓
silver_layer
        ↓
gold_layer
```

### Job Configuration
- **Schedule**: Daily automated execution
- **Retry policy**: 3 attempts on failure
- **Compute**: Serverless / Job cluster
- **Source**: Git-integrated (runs from `main` branch)
- **Version control**: Each execution tied to specific commit SHA

---

## 🔄 Version Control & CI/CD Readiness

### Git Integration
- ✅ All notebooks version-controlled in GitHub
- ✅ Job execution linked to specific commits
- ✅ Reproducible pipeline runs
- ✅ Branch-based development workflow

### Production Safety
- Commit-based execution ensures consistency
- Rollback capability via Git history
- Immutable execution artifacts

---

## 🧠 Key Engineering Concepts Demonstrated

- ✅ **Medallion Architecture** – Industry-standard data lakehouse pattern
- ✅ **Delta Lake** – ACID transactions, time travel, schema evolution
- ✅ **Multi-Source Ingestion** – Mock data, MySQL, PostgreSQL connectivity
- ✅ **JDBC Connectivity** – Production database integration
- ✅ **Incremental Loading** – Watermark-based delta loads
- ✅ **Schema Enforcement** – Strong typing and validation
- ✅ **Data Quality Validation** – Automated quality gates
- ✅ **Window Functions** – Advanced SQL analytics
- ✅ **Business Metric Engineering** – Translating raw data to insights
- ✅ **Workflow Orchestration** – Automated DAG execution
- ✅ **Batch Processing** – Scheduled ETL jobs
- ✅ **Security Best Practices** – Secrets management
- ✅ **Version Control** – Git-based development
- ✅ **Modular Design** – Reusable configuration

---

## 📈 Business Use Case

### Problem Statement
A ride-hailing company needs to identify which cities to expand into next.

### Solution
This pipeline analyzes search error logs to:
1. **Identify** cities with high search demand
2. **Detect** supply-demand gaps (no service area, no drivers nearby)
3. **Rank** cities by expansion priority
4. **Support** data-driven expansion decisions

### Impact
- 📊 Quantified expansion opportunities
- 🎯 Prioritized market entry strategy
- 💰 Optimized resource allocation

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| **Platform** | Databricks |
| **Storage** | Delta Lake |
| **Processing** | Apache Spark (PySpark) |
| **Orchestration** | Databricks Workflows |
| **Version Control** | Git / GitHub |
| **Language** | Python |
| **Data Sources** | Mock Data / MySQL / PostgreSQL |
| **Connectivity** | JDBC (MySQL Connector, PostgreSQL Driver) |
| **Security** | Databricks Secrets |

---

## 📊 Sample Output

### Gold Layer – Top Expansion Candidates

| rank | city | total_searches | expansion_signal_count | signal_ratio_pct |
|------|------|----------------|------------------------|------------------|
| 1 | Boston | 1250 | 687 | 54.96 |
| 2 | Austin | 1180 | 623 | 52.80 |
| 3 | Portland | 1095 | 568 | 51.87 |
| 4 | Denver | 1032 | 531 | 51.45 |
| 5 | Seattle | 978 | 487 | 49.79 |



