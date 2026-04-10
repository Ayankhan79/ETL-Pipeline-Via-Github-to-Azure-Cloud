# 🔥 COMPLETE FLOW (Your Architecture)

## 🌐 0. Source Layer (Web / GitHub)

* Data is stored in **GitHub (CSV files)**
* Accessed via:

  * **Base URL**
  * **Relative URL**

---

# 🟢 1. Layer 1 — Ingestion (Raw Layer)

## Services Used:

* **Azure Data Factory (ADF)**
* **Azure Data Lake Storage Gen2**

## Steps:

### ✅ Infra Setup

* Resource Group ✔️
* Storage Account (Data Lake Gen2) ✔️
* Containers:

  * `layer1`
  * `layer2`
  * `layer3`

---

## 🔁 Static Pipeline (Pipeline 1)

### Flow:

1. **Copy Data Activity**
2. **Linked Services**

   * HTTPS → GitHub
   * ADLS Gen2 → Data Lake

### Source:

* Dataset: CSV via HTTPS
* Uses:

  * Base URL
  * Relative URL

### Sink:

* ADLS Gen2
* Path → `layer1`

### Execution:

* Debug → Data loaded
* Publish → Persist pipeline

---

## ⚠️ Problem:

* Many files → cannot manually change relative URL each time

---

## 🔁 Dynamic Pipeline (Pipeline 2)

### 🧠 Concept:

Pipeline becomes like a **function**
→ Inputs passed via parameters

---

## 🔧 Parameters Created:

### Source:

* `url` → dynamic relative URL

### Sink:

* `datasink` → folder path (layer1)
* `filesin` → filename

---

## 📂 JSON Config File

* Stored in Data Lake (new folder: `parameter`)
* Contains:

  * File URLs
  * Output paths

---

## 🔄 Automation Logic:

### Activities:

1. **Lookup**

   * Reads JSON config

2. **ForEach**

   * Iterates over JSON items

3. **Copy Data**

   * Runs for each file dynamically

### Flow:

```
Lookup → ForEach → Copy Data
```

### Result:

✅ All GitHub files ingested into **Layer 1**

---

# 🟡 2. Layer 2 — Transformation (Processing Layer)

## Tool Used:

* **Azure Databricks (PySpark)**

---

## 🧠 Why Spark?

Because:

* Single system limitations:

  * CPU
  * Memory
  * Libraries
  * Dependencies

### Spark Solution:

* Distributed system (Cluster)
* Multiple machines (Nodes)

---

## ⚙️ Spark Architecture (Your Notes Correct 👇)

* **Cluster Manager (CM)**
* **Driver Program (DP)**
* **Worker Nodes (WN)**

### Flow:

```
Job → Driver → Tasks → Cluster Manager → Worker Nodes
```

---

## 🔐 Secure Access Setup

### Azure Entra ID:

* App Registration (`awsdatabrickpro1`)
* Collected:

  * Application ID
  * Directory ID
  * Client Secret

---

## 🔑 Permissions:

* Role assigned:

  * **Storage Blob Data Contributor**
* Given to:

  * Databricks App

---

## 💻 Databricks Steps:

1. Create Workspace
2. Launch
3. Create Cluster
4. Create Notebook

---

## 🔌 Connect to Data Lake

Using 5 configs:

* Storage account name
* App ID
* Tenant ID
* Client Secret

---

## 🔄 ETL in Databricks

### Steps:

1. **Read**

   * From Layer 1

2. **Transform**
   (You covered a LOT 👇)

   * select, filter, rename
   * joins, groupBy
   * window functions
   * missing value handling
   * encoding (OneHot, Label)
   * normalization
   * UDFs
   * PySpark SQL

3. **Write**

   * Format: **Parquet**
   * Destination: `layer2`

---

✅ Layer 2 Complete

---

# 🔵 3. Layer 3 — Data Warehouse (Serving Layer)

## Concept:

### Why not DB?

| Database     | Data Warehouse  |
| ------------ | --------------- |
| OLTP         | OLAP            |
| Current data | Historical data |
| Transactions | Analytics       |

---

## 🧠 Data Warehouse Components:

* **Load Manager** → Data ingestion
* **Warehouse Manager** → Storage + metadata + archive
* **Query Manager** → Query processing

---

## 🧱 Schema Design:

### ⭐ Star Schema

* **Fact Table**

  * Metrics
  * Foreign keys

* **Dimension Tables**

  * Descriptive data

---

## ⚙️ Implementation Option:

### ❌ Snowflake

* Expensive

### ✅ Azure Synapse Analytics (you chose)

---

## 🏗️ Synapse Setup:

1. Create Synapse Workspace
2. Open Synapse Studio

---

## 🔐 Permissions:

* Assign role:

  * Storage Blob Data Contributor
* To:

  * Synapse Managed Identity

---

## 🧾 In Synapse:

### Steps:

1. Create Database

2. Create Schema

3. Define:

   * Source (Layer 2)
   * Destination (Layer 3)

4. Define:

   * File Format → Parquet
   * Location → `layer3/employee`

---

## ▶️ Execution:

* Run SQL scripts

---

## 📦 Result:

* Data stored in:

  * Data Lake → `layer3`
  * Folder → `employee`
  * Format → Parquet

---

✅ Layer 3 Complete 🎉

---

# ❄️ Alternative: Snowflake

### Features:

* Auto schema detection
* SQL support
* Visualization
* **Time Travel** ⏳

  * Query past versions using Query ID

---

# 🔄 Final Orchestration

## Tool:

* **Apache Airflow**

### Purpose:

* Automate entire pipeline
* Run sequentially:

```
Layer1 → Layer2 → Layer3
```

### DAG:

* Define:

  * Start
  * Tasks
  * Dependencies

### With Docker:

* Containerized workflows

---

# 🚀 FINAL ARCHITECTURE (One Line)

👉 **GitHub → ADF (Dynamic Ingestion) → ADLS Layer1 → Databricks (PySpark Transform) → ADLS Layer2 → Synapse (Warehouse) → Layer3 → Airflow Orchestration**

---
