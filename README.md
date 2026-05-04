
# Retail AI Agent: SQL + Clustering + Forecasting

An end-to-end **agentic AI system** that enables users to query retail data using natural language and receive both **historical insights** and **forecasted demand predictions**.

---

## Overview

This project integrates a **Large Language Model (LLM)** with a **MySQL database** through an **n8n workflow** to:

* Convert natural language → SQL queries
* Retrieve structured retail data
* Analyze product clusters (DBSCAN)
* Serve **forecasted demand (Prophet + intermittent models)**
* Generate **business-friendly insights**

---

## Architecture

```text id="gk6m0g"
User Query (Natural Language)
        ↓
LLM (Intent Understanding + SQL Generation)
        ↓
Query Routing
(Historical → Sales | Forecast → Forecasts Table)
        ↓
n8n Workflow (AI Agent + MySQL Node)
        ↓
MySQL Database
   ├── sales (transactional data)
   ├── product_clusters (DBSCAN clusters)
   └── forecasts (predicted demand)
        ↓
SQL Execution
        ↓
LLM Interpretation
        ↓
Final Insight
```

---

##  Key Components

### 1 Sales Table

* Historical transactional data
* Fields: `date`, `description`, `units_sold`, `revenue`, `cluster_id`

---

### 2. Product Clusters

* Generated using **DBSCAN** on product embeddings
* Groups semantically similar products
* Example:

  * Cluster 0 → metal signs
  * Cluster 1 → decor items
  * Cluster 3 → bags

---

### 3. Forecasts Table

* Generated using:

  * **Prophet** (smooth/erratic demand)
  * **Croston / TSB** (intermittent demand)
* Fields:

  * `cluster_id`
  * `date`
  * `yhat` (prediction)
  * `yhat_lower`, `yhat_upper`

---

## Setup Instructions

### 1️⃣ Clone the Repository

```bash id="i3z2v4"
git clone <your-repo-url>
cd <repo-name>
```

---

### 2️⃣ Start n8n (Docker)

```bash id="qv0jv3"
docker compose up -d
```

Open n8n:

```id="dzxq5y"
http://localhost:5678
```

---

### 3️⃣ Set Up MySQL

Create database:

```sql id="cz6f7g"
CREATE DATABASE retail_forecasting;
```

---

### 4️⃣ Create Tables

#### Sales

```sql id="r3gwr0"
CREATE TABLE sales (
    date DATE,
    description TEXT,
    units_sold FLOAT,
    revenue FLOAT,
    avg_price FLOAT,
    cluster_id INT
);
```

#### Product Clusters

```sql id="8s5o1c"
CREATE TABLE product_clusters (
    description TEXT,
    cluster_id INT
);
```

#### Forecasts

```sql id="q5d0bl"
CREATE TABLE forecasts (
    cluster_id INT,
    date DATE,
    yhat FLOAT,
    yhat_lower FLOAT,
    yhat_upper FLOAT
);
```

---

### 5️⃣ Import Data

Import your CSVs:

* `sales_clean.csv`
* `description_clusters.csv`
* `forecasts.csv`

Using:

* MySQL Workbench
* or:

```sql id="36l2y8"
LOAD DATA INFILE 'forecasts.csv'
INTO TABLE forecasts
FIELDS TERMINATED BY ','
IGNORE 1 ROWS;
```

---

### 6️⃣ Configure n8n Workflow

In n8n:

1. Create workflow with:

   * **Chat Trigger Node**
   * **AI Agent Node (LLM)**
   * **MySQL Node**

2. Add your **MySQL credentials**

3. Set the **AI system prompt** to include:

   * sales + clusters + forecasts tables
   * routing logic (historical vs forecast)

4. Connect nodes:

```text id="9d7fx2"
Chat Trigger → AI Agent → MySQL → AI Response
```

5. Activate workflow

---

## Example Queries

```text id="1twtxj"
Which cluster generates the most revenue?
Show revenue for each cluster
Give examples of products in each cluster
What is the predicted demand for cluster 3 next week?
```
## Group Info

Team Name - ZAK Attack
```
Kaushiki Singh
Angela Yang
Zachary Balgut Tan
```
---

## Summary

An AI-powered analytics system that turns natural language into SQL and delivers cluster-level insights and forecasts.
