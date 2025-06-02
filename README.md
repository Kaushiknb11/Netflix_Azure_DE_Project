# Netflix Azure Data Engineering Project

## Overview

This is a real-world, end-to-end data engineering project that simulates a production-grade pipeline for managing Netflix content data using **Azure Data Factory**, **Databricks**, and **Delta Lake with Medallion Architecture**. It covers data ingestion, transformation, and streaming using best practices for modularity, scalability, and automation.

The project showcases:

- Dynamic pipelines with data validation  
- Incremental data ingestion using Autoloader  
- Parameterized notebooks and workflows  
- Delta Live Tables (DLT) in Gold Layer  
- Unity Catalog-based governance  
- Optional integration with Power BI and Synapse  

---

## Architecture

                           +------------------+
                           |   GitHub (CSV)   |
                           +--------+---------+
                                    |
                                    v
                        +--------------------------+
                        |  Azure Data Factory (ADF)|
                        |  (HTTP Connector + Datasets) |
                        +--------------------------+
                                    |
                                    v
                 +-----------------------------------------+
                 |   Azure Data Lake Storage Gen2 - Raw    |
                 |           (Bronze Layer)                |
                 +-----------------------------------------+
                                    |
                                    v
               +--------------------------------------------+
               |     Databricks Autoloader (Incremental)    |
               +--------------------------------------------+
                                    |
                                    v
                +-------------------------------------------+
                | Azure Data Lake Storage Gen2 - Cleaned    |
                |           (Silver Layer)                  |
                |   (Transformations using Spark SQL)       |
                +-------------------------------------------+
                                    |
                                    v
                +-------------------------------------------+
                | Azure Data Lake Storage Gen2 - Serving    |
                |           (Gold Layer)                    |
                | (Delta Live Tables + STAR SCHEMA)         |
                +-------------------------------------------+
                        |                         |
                        v                         v
           +------------------------+   +-----------------------+
           | Azure Synapse (Optional)|   | Power BI (Reporting) |
           +------------------------+   +-----------------------+


---

## Tools & Technologies Used

| Category              | Tools / Frameworks                                 |
|-----------------------|-----------------------------------------------------|
| **Cloud Platform**    | Microsoft Azure (Data Factory, ADLS Gen2, Synapse) |
| **Big Data Processing**| Databricks, Spark Structured Streaming             |
| **Storage**           | Delta Lake (Bronze, Silver, Gold)                  |
| **Automation & ETL**  | Azure Data Factory, Databricks Workflows           |
| **Governance**        | Unity Catalog, Azure RBAC                          |
| **Monitoring**        | ADF Alerts & Metrics, Logging with Checkpoints     |
| **Data Format**       | CSV, Delta                                          |
| **Optional Analytics**| Power BI                                            |

---

## Dataset

- **Netflix Movies and TV Shows Dataset**
- **Source**: Public GitHub repository
- **Master data**: Titles, Date Added, Type, Rating, Cast, Country, Categories
- **Lookup Tables**: Directors, Cast, Country, Category

---

## 🛠️ Key Features

### Azure Data Factory Pipelines
- HTTP connector to pull data from GitHub  
- Dynamic parameterized datasets  
- Copy activity wrapped in `ForEach` loop using pipeline parameter array  
- Data validation using `Exists` checks  
- Web activity to fetch metadata  

### Databricks Autoloader
- Incremental ingestion via `cloudFiles`  
- Schema inference and evolution  
- Checkpointing and exactly-once semantics  
- Writes to Delta format in the Bronze layer  

### Medallion Architecture
- **Bronze**: Raw ingestion via Autoloader  
- **Silver**: Data cleaning and basic transformation  
- **Gold**: Business-level aggregation via DLT  

### Unity Catalog & Access Control
- Catalog creation with admin roles  
- External locations for ADLS containers  
- Access Connector + Credential wrapping for scoped data access  

### Delta Live Tables (DLT)
- Gold tables implemented via DLT pipelines  
- Configured with Unity Catalog schemas  
- Real-time, scalable transformations  

### Optional Integration
- Data piped into Azure Synapse Analytics  
- Power BI data source connectivity shown (optional dashboarding)  

---

## Validation & Monitoring

- ADF pipeline monitor tab tracks every run  
- Alerts configured via ADF Alert Rules  
- Data validation step prevents downstream pipeline execution if data is missing  
- Schema inference stored via RocksDB in checkpoint locations  

---

## Getting Started

### Instructions
1. Clone the repository and set up Azure resources:
   - ADLS Gen2
   - Azure Data Factory
   - Databricks Workspace
2. Upload dataset files or configure HTTP pipeline  
3. Build medallion layer containers: `raw`, `bronze`, `silver`, `gold`  
4. Set up Unity Catalog, Access Connector, and credentials  
5. Run ADF pipeline to ingest files into Bronze layer  
6. Execute Databricks notebooks for Autoloader ingestion  
7. Transform and write to Silver & Gold using parameterized notebooks and workflows  
8. Configure DLT pipelines for the final layer  

---

## Learning Outcomes

- Understand incremental data processing using Autoloader  
- Implement medallion architecture in a production-style setup  
- Deploy parameterized ETL pipelines in ADF  
- Apply governance using Unity Catalog  
- Build real-time data engineering pipelines  
- Work with Delta Lake and Databricks workflows  



