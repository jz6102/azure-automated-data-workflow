# Automated Data Workflow using Azure Data Factory

This project is a proof-of-concept demonstrating a simple, automated data pipeline. It transfers and transforms structured data from a source database (Azure SQL) to a destination analytics store (Azure Blob Storage) using **Azure Data Factory**.

## Architecture

The pipeline follows a simple Extract, Transform, Load (ETL) process:



1.  **Source:** An Azure SQL Database containing sample product data.
2.  **Pipeline (Azure Data Factory):** A "Copy data" activity connects to the source, runs a SQL query to select and filter specific columns (the "transform" step), and moves the result.
3.  **Sink (Destination):** The transformed data is saved as a `products.csv` file in an Azure Blob Storage container.

## Tech Stack

- **Orchestration:** Azure Data Factory (ADF)
- **Data Source:** Azure SQL Database
- **Data Destination:** Azure Blob Storage
- **Transformation Logic:** SQL
- **Authentication:** System-Assigned Managed Identity

## How to Replicate

1.  Set up the required Azure resources: an Azure SQL Database (with sample data), an Azure Storage Account, and an Azure Data Factory instance.
2.  Configure the SQL Server for Entra-only authentication and set the Data Factory's Managed Identity as a server admin.
3.  Configure the SQL Server's networking firewall to "Allow Azure services and resources to access this server".
4.  In ADF, create the Linked Services, Datasets, and Pipeline as described, using the SQL query from the `source_query.sql` file in this repository.
