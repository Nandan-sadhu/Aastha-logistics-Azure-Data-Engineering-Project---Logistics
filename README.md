# Aastha-logistics-Azure-Data-Engineering-Project---Logistics
This project demonstrates the design and implementation of an end-to-end Data Engineering platform for a logistics company. The system integrates data from multiple heterogeneous sources including CSV files, HTTPS APIs, and on-premise TMS (Transport Management System).

The pipeline performs ELT/ETL operations using Azure services to transform raw data into business-ready datasets for Power BI dashboards and Machine Learning use cases.

Data Sources
Operational Data (Trip details, truck movement, fuel usage)
HR Data (Driver details, attendance, payroll)
Financial Data (Revenue, expenses, invoices)
CSV Files (Manual uploads from branches)
HTTPS APIs (External integrations)
On-Prem TMS System (Transport Management System database)

## Architecture Overview
# Logistics Data Engineering Project - End-to-End ELT Pipeline

## Architecture Diagram
![Architecture Diagram](Screeenshots/Architecture-Diagram.png)


## Flow Summary

* Data is ingested using **ADF with incremental load, CDC, and watermark logic**
* Stored in **Bronze layer (raw data)**
* ADF triggers **Databricks notebooks for processing**
* Cleaned data moves to **Silver layer**
* Business transformations, **SCD1 & SCD2**, and modeling happen in **Gold layer**
* Final data is consumed by **Power BI for reporting**



1. Data Ingestion
Built pipelines in Azure Data Factory

Integrated:
On-prem TMS via Self-hosted Integration Runtime
APIs via HTTP connectors
CSV files shared via azure gen2 Storage container daily basis
Implemented parameterized pipelines and scheduling

2. Data Storage (Medallion Architecture)
Raw Layer → Stores original data
Clean Layer → Standardized & validated data
Curated Layer → Business-ready datasets

3. Data Transformation (ELT/ETL)
Used Azure Databricks (PySpark) for transformations:
Data cleaning (nulls, duplicates)
Schema standardization
Joins across operational, HR, and finance data
Aggregations & KPI calculations

4. Business Use Cases
Operational Analytics
Fleet utilization
Trip efficiency
Fuel consumption tracking

HR Analytics
Driver attendance & performance
Salary vs productivity

Financial Analytics
Revenue vs cost analysis
Profitability by route

Advanced Analytics (ML Ready) future usecase
Demand forecasting
Route optimization
Cost prediction

5. Reporting Layer
Built Power BI dashboards for:
      Real-time operational monitoring
      Financial performance tracking
    HR insights

## Data
3 types of data
1. Ms SQL server- TMS data (onprem) HR, Operational data, financial
2. csv files- daily details Provided by site supervisor by csv files
3. API HTTPS- website live data, IOT data

## data Orchestration and how data flow
data is extracted from multiple source using Azure data factory and moved to data bronze layer directly without any transformation, 
Need 
a. linked services
b. IR- Self hosted (onprem), Auto resolve (Default Azure IR)
c. source- All diffent sources
d. Target- Bronze container

How the data is moved to bronze layer
      1. Onprem data is implemeted using CDC, incremtal load
      2. csv files Metadata based pipeline, selecting specific container and  event based trigger
      3. API calls, we are getting multiple small files, impliment batch processing on a 30 minutes batch due           to small file problem, use watermark based incrmental data load

Now we extract the data from bronze, Clean and tranform the data, optimize the pipeline move to silver layer parque file to reduce storage cost.
use ADF for pipeline contol flow and use databricks noteooks for transformations and cleaning.
(alternate we can also use auto loader, merge logic)

Now the data is clean and standardized in silver layer its time to move the data to gold layer. The data is extracted by Merge logic (upsert incemental ) and converted to fact and dimention tales, implement scd-1, scd-2
where needed finally store to the desired gold layer locations in delta tables.


## Key Features
Multi-source data integration
ELT + ETL hybrid processing
Medallion architecture
Scalable cloud-based solution
Supports BI + Machine Learning use cases
Automated and parameterized pipelines

## Skills Demonstrated
Azure Data Factory (ADF Pipelines, Integration Runtime)
Azure Databricks (PySpark)
Data Modeling & Transformation
SQL
Power BI
ELT/ETL Design
Cloud Data Engineering azure-ADf, synapse, databricks, powerbi

## Future Enhancements
Real-time streaming using Event Hub / Kafka
Incremental loading & CDC
CI/CD using Azure DevOps
Advanced ML pipelines integration
Data quality monitoring framework

## Author
Nandan Sadhu
