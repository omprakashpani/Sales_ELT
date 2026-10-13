# Sales_ELT

**Sales Data ELT Pipeline using Databricks, PySpark, Spark SQL, and Delta Lake**

## 📌 Overview

This project demonstrates an end-to-end ELT (Extract, Load, Transform) pipeline built using Databricks, PySpark, Spark SQL, and Delta Lake.

The pipeline processes raw sales transaction data, performs data cleaning and transformation, and generates business-ready datasets for sales and profit analysis.

It follows the **Medallion Architecture**, consisting of Bronze, Silver, and Gold layers.

## 🏗️ Architecture

```text
                  sales_data.csv
                        |
                        v
              +-------------------+
              |   Bronze Layer    |
              |   bronze_sales    |
              |   Raw Data        |
              +-------------------+
                        |
                        v
              +-------------------+
              |   Silver Layer    |
              |   silver_sales    |
              | Cleaned Data      |
              +-------------------+
                        |
                        v
              +-------------------+
              |    Gold Layer     |
              | gold_sales_by_region
              | Aggregated Data   |
              +-------------------+
                        |
                        v
              +-------------------+
              |   Spark SQL       |
              | Business Analytics|
              +-------------------+
```

## 📂 Dataset

The project uses `sales_data.csv`, a sample sales dataset containing information about orders, products, regions, prices, discounts, sales, and profits.

**Dataset columns include:**

- Order ID
- Order Date
- Region
- State
- City
- Category
- Sub-Category
- Product Name
- Quantity
- Unit Price
- Discount
- Sales
- Profit

The dataset contains sample data-quality issues, such as duplicate records and missing city values, to demonstrate data cleaning techniques.

## ⚙️ Pipeline Workflow

### 1. Extract

The CSV file is loaded into a PySpark DataFrame using Databricks.

- Read the CSV file.
- Identify column names and data types.
- Inspect the data and record count.

### 2. Bronze Layer — Raw Data

**Table:** `bronze_sales`

The original sales data is stored in Delta format.

Key operations:
- Load raw sales records.
- Preserve the source data for future processing.
- Store data as a Delta table.

### 3. Silver Layer — Data Cleaning and Transformation

**Table:** `silver_sales`

The Silver layer improves data quality and prepares the dataset for analysis.

Transformations performed:

- Removed duplicate records.
- Trimmed unnecessary whitespace from product names.
- Replaced missing city values with `Unknown`.
- Converted date and numeric columns into appropriate data types.
- Filtered records with missing order IDs or sales values.
- Removed records with negative sales.
- Created a derived `Profit_Margin` column.

### 4. Gold Layer — Business Aggregation

**Table:** `gold_sales_by_region`

The Gold layer generates aggregated sales metrics for business reporting.

Metrics calculated:

- Total sales by region.
- Total profit by region.
- Distinct order count by region.

The resulting Delta table can be used for reporting, dashboards, and business analysis.

### 5. Spark SQL Analytics

Spark SQL is used to analyze sales and profit by product category.

Example query:

```sql
SELECT
    Category,
    ROUND(SUM(Sales), 2) AS Total_Sales,
    ROUND(SUM(Profit), 2) AS Total_Profit
FROM silver_sales
GROUP BY Category
ORDER BY Total_Sales DESC;
```

This query identifies product categories based on total sales and shows their corresponding profit.

## 🗄️ Delta Tables Created

| Table Name | Description |
|---|---|
| `bronze_sales` | Raw sales data stored in Delta format |
| `silver_sales` | Cleaned and transformed sales records |
| `gold_sales_by_region` | Aggregated regional sales and profit metrics |

## 🛠️ Technologies Used

- **Databricks:** Data engineering and analytics platform.
- **PySpark:** Distributed data processing using Python.
- **Spark SQL:** SQL-based data querying and aggregation.
- **Delta Lake:** Reliable data storage with transaction support.
- **Medallion Architecture:** Layered data processing design.

## 📚 Concepts Demonstrated

- ELT pipeline development
- Bronze, Silver, and Gold architecture
- Data cleaning and preprocessing
- Null-value handling
- Duplicate removal
- Data type conversion
- DataFrame transformations
- Aggregations and grouping
- Delta table creation
- SQL-based analytics
- Data quality validation

## 🚀 How to Run the Project

1. Open your Databricks workspace.
2. Create a Python notebook.
3. Upload `sales_data.csv` to a location accessible to your Databricks compute.
4. Update the file path in the notebook.
5. Run the cells in the following order:
   - Load the CSV file.
   - Create the Bronze table.
   - Clean and transform the data to create the Silver table.
   - Aggregate the data to create the Gold table.
   - Execute the Spark SQL analytics queries.
6. Review the generated tables and analytical results.

**Note:** Your Databricks catalog and schema must allow table creation. If necessary, use fully qualified table names such as `main.default.bronze_sales`, replacing the example catalog and schema with your own.

## 📊 Expected Outcome

After executing the pipeline, you will have:

- Raw sales data stored in the Bronze layer.
- Cleaned and validated records in the Silver layer.
- Regional sales and profit summaries in the Gold layer.
- SQL queries for product-category analysis.
- A reusable foundation for further data engineering tasks.

## 🔮 Future Enhancements

- Implement incremental data processing using Delta Lake `MERGE`.
- Add automated data quality checks and rejected-record handling.
- Schedule pipeline execution using Databricks Jobs.
- Develop interactive dashboards using Gold-layer tables.
- Add logging, monitoring, and failure alerts.
- Process larger datasets from cloud storage.

## 👨‍💻 Project Summary

This project demonstrates the implementation of an ELT pipeline using Databricks, PySpark, Spark SQL, and Delta Lake. By applying Medallion Architecture, the pipeline separates raw data ingestion, data cleaning, and business-level aggregation into distinct layers, making the workflow easier to understand, maintain, and extend.

---

**Project:** Sales Data ELT Pipeline  
**Domain:** Data Engineering  
**Tools:** Databricks | PySpark | Spark SQL | Delta Lake
