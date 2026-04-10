# ETL-Pipeline-Via-Github-to-Azure-Cloud

# 🚀 Azure End-to-End Data Engineering Pipeline

## 📌 Overview

This project demonstrates a **complete end-to-end data engineering pipeline** built on Azure. It ingests data from GitHub, processes it using distributed computing, and stores it in a data warehouse for analytics.

---

## 🏗️ Architecture

```mermaid
flowchart LR
    A[GitHub CSV Data] --> B[Azure Data Factory]
    B --> C[ADLS Gen2 - Layer 1 (Raw)]
    C --> D[Azure Databricks (PySpark)]
    D --> E[ADLS Gen2 - Layer 2 (Processed)]
    E --> F[Azure Synapse Analytics]
    F --> G[ADLS Gen2 - Layer 3 (Warehouse)]
    G --> H[Analytics / BI Tools]

    subgraph Orchestration
        I[Apache Airflow DAG]
    end

    I --> B
    I --> D
    I --> F
```

---

## 🧱 Tech Stack

* **Ingestion:** Azure Data Factory
* **Storage:** Azure Data Lake Storage Gen2
* **Processing:** Azure Databricks (PySpark)
* **Warehouse:** Azure Synapse Analytics
* **Orchestration:** Apache Airflow
* **Source:** GitHub (CSV files)

---

## 🔄 Data Flow

### 🟢 Layer 1 — Raw Data (Ingestion)

* Data is fetched from GitHub using **ADF pipelines**
* Implemented:

  * Static pipeline (initial)
  * Dynamic pipeline using:

    * Parameters
    * JSON config
    * Lookup + ForEach
* Stored in:

  ```
  ADLS → layer1/
  ```

---

### 🟡 Layer 2 — Processed Data (Transformation)

* Data processed using **PySpark in Databricks**
* Transformations include:

  * Filtering, joins, aggregations
  * Missing value handling
  * Encoding (OneHot, Label)
  * Window functions
* Output format:

  ```
  Parquet
  ```
* Stored in:

  ```
  ADLS → layer2/
  ```

---

### 🔵 Layer 3 — Data Warehouse (Serving)

* Data loaded into **Azure Synapse Analytics**
* Structured using:

  * Star Schema

    * Fact Tables (metrics)
    * Dimension Tables (descriptive)
* Stored in:

  ```
  ADLS → layer3/
  ```

---

## ⚙️ Key Features

✅ Dynamic data ingestion from multiple files
✅ Distributed data processing using Spark
✅ Secure access via Azure Entra ID & Managed Identity
✅ Scalable storage using Data Lake Gen2
✅ Analytical layer using Synapse
✅ End-to-end orchestration with Airflow

---

## 🔐 Security & Access Control

* Role-based access:

  * Storage Blob Data Contributor
* Authentication via:

  * App Registration (Client ID, Secret, Tenant ID)
* Managed Identity used for Synapse access

---

## 📂 Project Structure

```
project/
│
├── data/
│   ├── layer1/        # Raw data
│   ├── layer2/        # Processed data
│   └── layer3/        # Warehouse data
│
├── pipelines/
│   ├── pipeline1.json # Static pipeline
│   └── pipeline2.json # Dynamic pipeline
│
├── config/
│   └── files.json     # Input configuration
│
├── notebooks/
│   └── transformation.py
│
└── airflow/
    └── dag.py
```

---

## 🔄 Orchestration (Airflow DAG)

```python
start >> ingest_data >> transform_data >> load_to_warehouse >> end
```

---

## 📊 Concepts Used

* ETL Pipelines
* Distributed Computing (Spark)
* Data Lake Architecture (Medallion: Bronze, Silver, Gold)
* OLTP vs OLAP
* Star Schema Design

---

## 🚀 Future Improvements

* Implement **Delta Lake**
* Add **incremental loading**
* Enable **data quality checks**
* Integrate **monitoring & alerting**
* Add **CI/CD pipeline**

---

## 📌 Conclusion

This project showcases a **production-style scalable data pipeline** handling:

* Data ingestion
* Transformation
* Storage
* Analytics

Built with modern cloud-native tools, it reflects real-world data engineering practices.

---
