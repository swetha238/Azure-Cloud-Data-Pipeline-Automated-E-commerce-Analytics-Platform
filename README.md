# Azure Data Engineering Pipeline
## End-to-End ETL Pipeline for E-commerce Analytics



## 📊 Project Overview

This project demonstrates a comprehensive **end-to-end data engineering pipeline** built on Microsoft Azure, designed to process e-commerce data from a SQL Server database through a modern data lake architecture. The pipeline transforms raw sales data from AdventureWorksLT2017 (a bicycle and accessories e-commerce company) into actionable business insights.

### 🎯 Business Use Case
- **Company**: AdventureWorks Cycles (fictional bicycle manufacturer and retailer)
- **Industry**: E-commerce - Bicycles, accessories, and outdoor gear
- **Data Source**: AdventureWorksLT2017 database with SalesLT schema
- **Goal**: Transform raw sales data into clean, analytics-ready datasets for business intelligence

---

## 🏗️ Architecture Overview

The pipeline follows a modern **medallion architecture** (Bronze → Silver → Gold) using Azure services:

```
On-Premises SQL Server → Azure Data Factory → Data Lake (Bronze)
                                    ↓
Azure Databricks → Data Lake (Silver) → Data Lake (Gold)
                                    ↓
Azure Synapse Analytics → Power BI Dashboard
```

### **Data Flow:**
1. **Ingestion**: Raw data from SQL Server to Bronze layer
2. **Transformation**: Data cleaning and standardization in Silver layer
3. **Enrichment**: Business logic and naming conventions in Gold layer
4. **Analytics**: Views in Synapse for Power BI consumption

---

## 🛠️ Technology Stack

| Service | Purpose | Technology |
|---------|---------|------------|
| **Azure Data Factory** | Data orchestration and ETL | Self-hosted integration runtime |
| **Azure Data Lake Storage Gen2** | Data storage (3-layer architecture) | Hierarchical namespace enabled |
| **Azure Databricks** | Data transformation and processing | PySpark, Python |
| **Azure Synapse Analytics** | Data warehousing and querying | Serverless SQL pool |
| **Power BI** | Data visualization and reporting | Interactive dashboards |
| **Azure Key Vault** | Security and credential management | Managed identities |

---

## 📁 Project Structure

```
azure-data-engineering-pipeline/
├── 📂 data/                              # Sample database
│   └── AdventureWorksLT2017.bak         # E-commerce sample database backup
├── 📂 docs/                             # Comprehensive documentation
│   ├── SETUP_GUIDE.md                   # Detailed setup instructions
│   ├── DEPLOYMENT.md                    # Azure deployment guide
│   └── PROJECT_STRUCTURE.md             # Project structure documentation
├── 📂 notebooks/                        # Databricks Python notebooks
│   ├── storagemount.py                  # Storage mount configuration
│   ├── bronze_to_silver.py              # Bronze to silver transformation
│   └── silver_to_gold.py                # Silver to gold transformation
├── 📂 scripts/                          # SQL scripts
│   └── CreateSQLServerlessView_gold.sql # Synapse view creation procedure
├── 📂 screenshots/                      # Architecture diagrams and screenshots
├── 📂 visualizations/                   # Power BI dashboard
│   └── PowerBI.pbix                     # Interactive business dashboard
├── 📄 config.json                       # Azure resource configuration
├── 📄 config_template.json              # Configuration template
├── 📄 .gitignore                        # Git ignore rules
├── 📄 LICENSE                           # MIT License
└── 📄 README.md                         # This file
```

---

## 🔄 Data Pipeline Flow

### 1. **Data Ingestion (Bronze Layer)**
- **Source**: On-premises SQL Server with AdventureWorksLT2017 database
- **Method**: Azure Data Factory with Self-Hosted Integration Runtime
- **Storage**: Azure Data Lake Storage Gen2 (bronze container)
- **Format**: Parquet files
- **Tables Processed**: Customer, Product, SalesOrderHeader, SalesOrderDetail, Address, ProductCategory, ProductModel

### 2. **Data Transformation (Silver Layer)**
- **Processing**: Azure Databricks with PySpark
- **Transformations**:
  - Date standardization to "yyyy-MM-dd" format
  - Data type conversions
  - Basic data cleaning
- **Storage**: Azure Data Lake Storage Gen2 (silver container)
- **Format**: Delta Lake tables

### 3. **Data Enrichment (Gold Layer)**
- **Processing**: Azure Databricks with PySpark
- **Transformations**:
  - Column naming convention: PascalCase → snake_case
  - Data aggregation and enrichment
  - Final data quality checks
- **Storage**: Azure Data Lake Storage Gen2 (gold container)
- **Format**: Delta Lake tables

### 4. **Data Serving (Analytics Layer)**
- **Processing**: Azure Synapse Analytics Serverless SQL Pool
- **Implementation**: Dynamic view creation for each table
- **Access**: Power BI direct query connection
- **Output**: Interactive business intelligence dashboards

---

## 📊 Dataset Information

### AdventureWorksLT2017 - E-commerce Database

**Business Domain**: Bicycle and accessories retail company

**Key Tables in SalesLT Schema**:

| Table | Description | Key Columns |
|-------|-------------|-------------|
| **Customer** | Customer information | CustomerID, FirstName, LastName, EmailAddress |
| **Product** | Product catalog | ProductID, Name, ProductNumber, StandardCost, ListPrice |
| **ProductCategory** | Product categories | ProductCategoryID, Name, ParentProductCategoryID |
| **ProductModel** | Product models | ProductModelID, Name, CatalogDescription |
| **SalesOrderHeader** | Sales orders | SalesOrderID, OrderDate, DueDate, ShipDate, CustomerID |
| **SalesOrderDetail** | Order line items | SalesOrderID, ProductID, OrderQty, UnitPrice, LineTotal |
| **Address** | Address information | AddressID, AddressLine1, City, StateProvince, PostalCode |

**Sample Business Scenarios**:
- Customer purchase history analysis
- Product performance metrics
- Sales trend analysis
- Geographic sales distribution
- Inventory management insights

---

## 🚀 Execution Steps

### Prerequisites
- Active Azure Subscription
- Azure CLI installed and configured
- Power BI Desktop
- SQL Server Management Studio (optional)
- Python environment

### 1. Infrastructure Setup
```bash
# Azure CLI login
az login
az account set --subscription "your-subscription-id"

# Create resource group
az group create --name "rg-dataengineering" --location "East US"

# Deploy storage account
az storage account create \
  --name "swethadataproject" \
  --resource-group "rg-dataengineering" \
  --location "East US" \
  --sku Standard_LRS \
  --kind StorageV2 \
  --hierarchical-namespace true
```

### 2. Data Pipeline Configuration
1. **Storage Mount Setup**: Execute `notebooks/storagemount.py` in Databricks
2. **Data Transformation**: Run `notebooks/bronze_to_silver.py`
3. **Data Enrichment**: Execute `notebooks/silver_to_gold.py`
4. **View Creation**: Run `scripts/CreateSQLServerlessView_gold.sql` in Synapse

### 3. Visualization Setup
1. Open `visualizations/PowerBI.pbix`
2. Update data source connections to your Synapse workspace
3. Refresh data and customize dashboards

---

## 🔧 Configuration

### Azure Resources Used
- **Storage Account**: `swethadataproject.dfs.core.windows.net`
- **Resource Group**: `rg-dataengineering`
- **Deployment**: `swethadataproject_1759430576096`
- **Containers**: bronze, silver, gold

### Environment Variables
Update `config.json` with your Azure resource details:
```json
{
  "azure_config": {
    "storage_account": "swethadataproject",
    "resource_group": "rg-dataengineering",
    "subscription_id": "your-subscription-id"
  }
}
```

---

## 📈 Key Features

### ✅ **Modern Data Architecture**
- Medallion architecture (Bronze → Silver → Gold)
- Delta Lake for ACID transactions
- Serverless SQL for cost optimization

### ✅ **Scalable Processing**
- PySpark for distributed data processing
- Auto-scaling Databricks clusters
- Parallel data transformation

### ✅ **Security & Governance**
- Azure Key Vault for credential management
- Managed identities for authentication
- RBAC for access control

### ✅ **Business Intelligence**
- Interactive Power BI dashboards
- Real-time data visualization
- Self-service analytics capabilities
