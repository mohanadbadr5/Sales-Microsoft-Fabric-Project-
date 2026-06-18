<img width="1301" height="732" alt="image" src="https://github.com/user-attachments/assets/013c7bdd-ba27-41f7-81f6-dfab393de9da" />

<img width="1918" height="877" alt="image" src="https://github.com/user-attachments/assets/c781ff6d-9c6d-4446-bd5e-dae34ecab5db" />

<img width="1918" height="875" alt="image" src="https://github.com/user-attachments/assets/3551a925-4cd1-4e30-a00b-bea46516b0e6" />


**End-to-End Sales Analytics Architecture on Microsoft Fabric & Power BI**

📌 Project Overview

This repository contains the complete implementation of an automated, cloud-based End-to-End Business Intelligence and Data Engineering solution built on top of the Microsoft Fabric SaaS platform. The project ingests raw internet-sourced sales data, processes it through multiple storage tiers (Lakehouse to Data Warehouse), and delivers a fully interactive, production-ready Power BI Dashboard running on the ultra-fast Direct Lake mode.

________________________________________

🏗️ Architectural Workflow & Pipeline Stages

The entire solution is orchestrated dynamically via a Data Factory Pipeline incorporating the following sequential steps:
1.	Environment Sanitization (File Clean-up): Automates the removal of stale *.csv metadata within the Lakehouse files zone before starting a new batch.
2.	Data Ingestion (Copy Activity): Extracts raw, delimited sales telemetry via a secure cloud connection and drops it into OneLake/Files/Data_Source.
3.	Data Transformation T1 (PySpark Notebook): A native Spark session parses raw CSV layers, dynamically creates structural Year and Month tracking values, and saves the refined payload as highly compressed Delta Lake Tables.
4.	Data Transformation T2 (Power Query / Dataflow Gen2): Performs column transformations, handles nested string splits to decouple buyer names into FirstName and LastName, and stages cleansed records.
5.	Data Integration (Stored Procedure / T-SQL): Executes an atomic SCD Type 1 update/upsert (Incremental Load). It populates a robust dimensional model (Star Schema) while calling native DATENAME features to store chronological text-based months directly inside the relational layer to optimize caching performance.
6.	Semantic Layer Refresh: Triggers an automated metadata-driven callback to refresh the Direct Lake semantic model for instantaneous analysis.

________________________________________

📊 Data Model (Star Schema)

The system eliminates complex Snowflake nesting in favor of a performant, decoupled Star Schema containing:
•	Fact_Sales: Centralized facts containing core measures (Quantity, UnitPrice, TaxAmount) alongside physical text timestamps.
•	Dim_Customer: Unique entity record containing non-enforced Primary Keys (PK_Dim_Customer) managing subscriber data.
•	Dim_Item: Unique product index catalog managing product stock listings.

________________________________________

📈 Dashboard Capabilities & Insights

The production report (Sales Monthly Report) delivers actionable executives-ready summaries:
•	Advanced Axis Sorting: Overcomes alphabetical sorting traps of text columns using precise underlying DAX chronological sorting models.
•	Smart Narratives: Context-aware NLP calculations displaying dynamic trend evaluations (e.g., highlighting steep Q4 velocity peaks and mid-year volume drops).
•	Funnel Optimizations: Clear funnel visualization showing product popularity rankings down to units sold.

________________________________________

🛠️ Technology Stack

•	Platform: Microsoft Fabric (SaaS Workspace)
•	Storage Engine: Azure OneLake, Delta Lake (Parquet)
•	Processing Engines: Apache Spark (PySpark), Dataflows Gen2 (M-Code), Synapse SQL Data Warehouse
•	Database Programming: T-SQL (Stored Procedures, Views, DDL/DML script handling)
•	Data Visualization & Analytics: Power BI Desktop, DAX, Direct Lake Connection

________________________________________

🚀 How to Deploy This Architecture

1.	Clone this repository and import the SQL initialization scripts found under the /scripts directory into your Synapse Warehouse Endpoint.
2.	Deploy the provided M-Code inside a new Fabric Dataflow Gen2.
3.	Upload the .ipynb file to your Fabric Data Engineering cluster.
4.	Wire the activities inside your Fabric Data Factory Pipeline based on the workflow layout shown in the architectural diagrams.
5.	Set up a regular automated orchestrator schedule under Pipeline Settings for automated triggers.

