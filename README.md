# Sales_ELT
Sales Data ELT Pipeline
Technologies: Databricks · PySpark · Spark SQL · Delta Lake · Medallion Architecture
Overview
This project demonstrates an end-to-end ELT pipeline built in Databricks. It loads sales transaction data, stores the raw data in a Bronze Delta table, cleans and validates it in the Silver layer, and creates business-ready regional sales summaries in the Gold layer.
Architecture
sales_data.csv
      |
      v
Bronze Layer — bronze_sales
Raw data stored in Delta format
      |
      v
Silver Layer — silver_sales
Deduplication, text cleanup, type conversion,
null handling, filtering, profit-margin calculation
      |
      v
Gold Layer — gold_sales_by_region
Sales, profit, and distinct order counts by region
      |
      v
Spark SQL analytics
Category-level sales and profit reporting
Dataset
The project uses sales_data.csv, a small Superstore-style sample dataset containing order dates, regions, cities, product categories, quantities, prices, discounts, sales, and profit.
The sample data includes a few data-quality issues (such as duplicate records and a missing city) to demonstrate cleaning steps.
Pipeline Steps
1. Extract: Read the CSV file into a PySpark DataFrame.
2. Load (Bronze): Save the source records to the bronze_sales Delta table.
3. Transform (Silver):
   - Remove duplicate rows.
   - Trim product-name whitespace.
   - Replace missing city values with Unknown.
   - Convert date and numeric fields to appropriate types.
   - Filter rows with missing order IDs or sales and negative sales.
   - Calculate Profit_Margin.
4. Transform and aggregate (Gold):
   - Group records by region.
   - Calculate total sales, total profit, and distinct order counts.
   - Save the output to gold_sales_by_region.
5. Analyze: Use Spark SQL to aggregate sales and profit by product category.
Tables Created
Table	Purpose
bronze_sales	Raw source records in Delta format
silver_sales	Cleaned and transformed sales records
gold_sales_by_region	Regional sales and profit summary


Tools and Concepts Demonstrated
- Databricks notebooks
- PySpark DataFrame API
- Spark SQL
- Delta Lake tables
- Medallion Architecture (Bronze, Silver, Gold)
- Data cleaning and quality checks
- Deduplication and null handling
- Aggregations and derived columns
How to Run
1. Open your Databricks workspace and create or open a Python notebook.
2. Upload sales_data.csv to a location accessible to your compute.
3. Update the CSV path in the first notebook cell to match the uploaded file's actual path.
4. Run the notebook cells in order:
   - Read the CSV.
   - Create bronze_sales.
   - Clean and save silver_sales.
   - Create gold_sales_by_region.
   - Run the Spark SQL analytics query.
5. Check the output tables and query results.
Note: The code uses saveAsTable(), so your selected catalog and schema must allow table creation. If necessary, qualify table names with your catalog and schema, for example main.default.bronze_sales. Use a catalog/schema you have permission to write to.

Example SQL Query
SELECT
    Category,
    ROUND(SUM(Sales), 2) AS Total_Sales,
    ROUND(SUM(Profit), 2) AS Total_Profit
FROM silver_sales
GROUP BY Category
ORDER BY Total_Sales DESC;
Notes and Limitations
- This is a learning/demo pipeline using a small sample dataset, not a production-scale deployment.
- The regional Gold table counts distinct order IDs. If an order contains multiple line items, Order_Count represents distinct orders, not raw rows.
- Profit_Margin is calculated as Profit / Sales * 100; rows with zero sales should be handled explicitly if they exist in a different dataset.
- The current example uses overwrite writes for simplicity. A production pipeline would typically add incremental ingestion, monitoring, error handling, and an appropriate Delta MERGE strategy.
Possible Enhancements
- Add automated data-quality assertions and quarantine invalid records.
- Implement incremental loads with Delta Lake MERGE.
- Schedule the notebook as a Databricks Job.
- Build a dashboard from the Gold tables.
- Add logging, alerting, and pipeline metrics.
Created as a practical project to learn Databricks, PySpark, Delta Lake, and ELT pipeline design.
