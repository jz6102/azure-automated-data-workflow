# Automated Data Workflow with Azure Data Factory

This project serves as a proof-of-concept demonstrating a robust, automated data pipeline built entirely on the Azure cloud platform. [cite_start]The core function of this project is to extract data from a transactional database, perform a simple transformation, and load it into a cloud storage solution, a common pattern in modern data engineering. [cite: 111, 112]

This repository contains the SQL query and the Infrastructure as Code (IaC) templates needed to replicate the project.

## 🏛️ Architecture

The pipeline follows a simple yet powerful Extract, Transform, Load (ETL) process orchestrated by Azure Data Factory.


1.  **Source (Extract):** An **Azure SQL Database** instance containing the `AdventureWorksLT` sample dataset serves as the source of our data.
2.  **Orchestration & Transformation (Transform):** An **Azure Data Factory** pipeline connects to the source database. A "Copy data" activity executes a custom SQL query to select specific columns and filter out unnecessary rows, performing the transformation in-transit. Authentication between the services is handled securely using a **System-Assigned Managed Identity**.
3.  **Sink (Load):** The transformed data is loaded into an **Azure Blob Storage** container as a `.csv` file, ready for analytics, reporting, or downstream consumption.

## 🛠️ Tech Stack

-   **Orchestration:** Azure Data Factory (ADF)
-   **Data Source:** Azure SQL Database
-   **Data Destination:** Azure Blob Storage
-   **Transformation Logic:** SQL
-   **Authentication:** Azure Active Directory (Entra ID) & System-Assigned Managed Identity
-   **Infrastructure as Code (IaC):** ARM Templates

## 📋 Project Workflow & Key Features

-   **Automated ETL Process:** The pipeline is fully automated to move data without manual intervention.
-   **In-Flight Transformation:** The SQL query filters the data during the copy activity, ensuring only relevant information (`ProductID`, `Name`, `Color`, `StandardCost`, `SellStartDate`) is moved to the destination.
-   **Secure, Password-less Authentication:** By using a Managed Identity, the Data Factory authenticates with the SQL Server securely through Azure's backbone without needing to store any passwords or secrets.
-   **Scalable Cloud Infrastructure:** All components are Azure-native services, allowing for easy scaling to handle larger data volumes.
-   **Reproducible Infrastructure:** The entire Data Factory's structure (pipelines, datasets, etc.) is exported as ARM templates located in the `/adf-arm-template` folder.

## 🚀 Setup and Replication Steps

To replicate this project, you will need an active Azure subscription. Follow these steps:

### 1. Azure Resource Provisioning
-   **Create a Resource Group:** Create a new resource group to hold all project components.
-   **Create Azure SQL Server & Database:**
    -   Provision a new Azure SQL Server.
    -   During setup, under "Authentication," select "Use Microsoft Entra-only authentication".
    -   Create an Azure SQL Database within this server. In the "Additional settings" tab, select "Sample" to load the `AdventureWorksLT` data.
-   **Create Azure Storage Account:** Provision a standard general-purpose v2 storage account. Create a container inside it named `output-data`.
-   **Create Azure Data Factory:** Provision a new Data Factory (V2) instance.

### 2. Configure Networking and Permissions
-   **Enable Data Factory's Managed Identity:**
    -   Navigate to your Data Factory resource in the Azure Portal.
    -   Go to the **Identity** tab under "Settings".
    -   Under the "System assigned" tab, switch the **Status** to **On** and save.
-   **Grant ADF Permissions on SQL Server:**
    -   Navigate to your SQL Server resource.
    -   Go to the **Microsoft Entra** tab.
    -   Click **"+ Set admin"** and search for your Data Factory's name. Select it to make it an admin on the server and save.
-   **Configure SQL Server Firewall:**
    -   Navigate to your SQL Server resource.
    -   Go to the **Networking** tab.
    -   Select the **"Selected networks"** option.
    -   Under "Exceptions," check the box for **"Allow Azure services and resources to access this server"** and save.

### 3. Build the Pipeline in Azure Data Factory
-   **Create Linked Services:**
    -   Create a Linked Service for **Azure Blob Storage**, pointing to the storage account you created.
    -   Create a Linked Service for **Azure SQL Database**. For "Authentication type," select **"System Assigned Managed Identity"**.
-   **Create Datasets:**
    -   Create a source dataset (`DS_AzureSQL_Products`) pointing to the `[SalesLT].[Product]` table using the SQL Linked Service.
    -   Create a sink dataset (`DS_BlobStorage_ProductsCSV`) pointing to the `output-data/products.csv` file in your Blob Storage. Ensure "First row as header" is checked and "Import schema" is set to "None".
-   **Create and Configure the Pipeline:**
    -   Create a new pipeline and add a **"Copy data"** activity.
    -   In the "Source" tab, select the SQL dataset and change the "Use query" option to **"Query"**. Paste the content from the `source_query.sql` file in this repository.
    -   In the "Sink" tab, select the Blob Storage dataset.
-   **Run and Publish:**
    -   Click **"Debug"** to test the pipeline run.
    -   Verify the `products.csv` file is created in your storage container.
    -   Click **"Publish all"** to save your work.

## 📂 Repository Structure
├── adf-arm-template/
│   ├── arm_template.json           # ARM Template for the ADF structure
│   └── arm_template_parameters.json # Parameters for the ARM Template
├── source_query.sql                # The SQL transformation query
└── README.md                       # This file

##  D-Contact

-   **Author:** Jaikanna B
-   [cite_start]**LinkedIn:** [linkedin.com/in/jaikanna777](https://www.linkedin.com/in/jaikanna777) [cite: 77]
-   [cite_start]**GitHub:** [github.com/jz6102](https://github.com/jz6102) [cite: 77]

