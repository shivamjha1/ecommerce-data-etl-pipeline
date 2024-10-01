# fEcommerce-etl

1
DATABRICKS PROJECT REPORT
Project TitleEnd-to-End ETL Pipeline Implementation and Data Analytics in 
Databricks for Retail Sales Insights.
Prepared byShivam Kumar Jha
Date-
2
Table of Contents
1. Introduction
1.1 Project Overview
1.2 Dataset Description
2. Setup and Initial Data Load
2.1 Task Overview
2.2 Methodology
3. Bronze Layer - Raw Data Ingestion
3.1 Task Overview
3.2 Methodology
4. Data Quality Checks
4.1 Task Overview
4.2 Methodology
5. Silver Layer - Data Transformation and Enrichment
5.1 Task Overview
5.2 Methodology
6. Gold Layer - Aggregation, Filtering, and Reporting
6.1 Task Overview
6.2 Methodology
7. Data Validation and Auditing
7.1 Task Overview
7.2 Methodology
8. Final Output, Reporting, and Dashboarding
8.1 Task Overview
8.2 Methodology
8.3 Dashboard
9. Conclusion
9.1 Project Summary
9.2 Future Work
 
3
1. Introduction
1.1 Project Overview
This project involved the implementation of a full ETL (Extract, Transform, Load) pipeline 
using Databricks. The objective was to upload and process a retail sales dataset, 
including orders, customers, order items and products, perform data ingestion of these 
CSV files, clean the data, apply transformations, and generate meaningful insights 
through data aggregation and reporting.
1.2 Dataset Description
Four datasets were used in this project:
• Orders Dataset (orders.csv): Contains details about customer orders, including 
order ID, order date, customer ID, order status, etc.
• Customers Dataset (customers.csv): Provides information about customers, 
including customer ID, customer name, and location details.
• Order Items Dataset (order_items.csv): Lists the individual items within each 
order, including order item ID, order ID, product ID, quantity, and price.
• Products Dataset(products.csv): Lists the name, and details of all the products 
like product id, product name and category.
2. Setup and Initial Data Load
2.1 Task Overview
• Objective: Upload the dataset into Databricks and load it into a DataFrame.
• Steps:
1. Logged into Databricks Community Edition.
4
2. Created a new notebook.
3. Uploaded the orders.csv, customers.csv, order_items.csv and product.csv
datasets to Databricks using the DBFS(Databricks File System).
4. Loaded each dataset into separate DataFrames, ensuring proper parsing 
of data.
2.2 Methodology
• DBFS: The Databricks File System (DBFS) is a distributed file system that allows 
you to store and access files in Databricks. It abstracts storage, making it easy to 
manage data across different clusters and notebooks.
• DataFrame Loading: Used the spark.read.csv method to load the data into 
DataFrames. Ensured that all columns were correctly parsed, and appropriate 
data types were assigned.
3. Bronze Layer - Raw Data Ingestion
3.1 Task Overview
• Objective: Ingest the raw data into the Bronze layer, handling schema evolution 
and semi-structured data.
• Steps:
1. Loaded the dataframes from raw data folder using spark.read.csv.
2. Defined the structure and schema of all the columns of dataset
3. Checked for any null or duplicated values.
4. Added the current timestamp column.
5. Saved the DataFrames as Delta tables in the Bronze layer.
6. Implemented schema enforcement using Delta Lake to ensure data 
integrity during ingestion.
3.2 Methodology
5
• Schema Enforcement: Ensured that the schema was strictly enforced during the 
ingestion process. This step involved verifying column data types and ensuring no 
critical data was missing.
• Data Lineage: Added Created at column using current_timestamp().
• Writing in bronze layer as delta tables: Used spar.write.format(‘delta’) to write 
the well defined datasets into the bronze directory.
• Bronze Table Structure: Saved the data in the following paths:
o /bronze/orders
o /bronze/customers
6
o /bronze/prod
o /bronze/order_item
7
4. Data Quality Checks
4.1 Task Overview
• Objective: Implement data quality checks to identify and log issues before 
moving data to the Silver layer.
• Steps:
1. Checked for null values, duplicates, and data type mismatches in each 
dataset.
2. Logged any detected issues into a separate Delta table for auditing.
4.2 Methodology
• Data Quality Checks: Utilized PySpark functions such as dropna(), 
dropDuplicates(), and cast() to identify and handle potential issues in the data.
• Null data replaced: In the customers data, replaced the null values in the gender 
column with ‘Unknown’ and the email field null values is replaced with 
<username><id>@gmail.com
• Auditing: Created an audit Delta table to store records of data quality issues for 
transparency and tracking.
5. Silver Layer - Data Transformation and Enrichment
5.1 Task Overview
• Objective: Perform complex transformations, enrich the data and perform logical 
joins.
• Steps:
1. Read data from the Bronze tables.
8
2. Cleaned the data and joined tables to create a comprehensive dataset, 
including customer order history.
3. Join the tables logically.
4. Saved the transformed data as Delta tables in the Silver layer.
5.2 Methodology
• Data Cleaning: Applied techniques such as handling missing values, correcting 
data formats, and standardizing categorical data.
• Understand the relation: Understood the relation between different tables and 
on which common columns they could be joined.
• Table Joins: Performed joins between orders, customers, products and 
order_items tables to create a unified view of customer orders.
• Silver Table Structure: Saved the data in the following paths:
o /silver/customer_orders
o /silver/total_orders
o /silver/total_products
o /silver/total_data
6. Gold Layer - Aggregation, Filtering, and Reporting
6.1 Task Overview
9
• Objective: Aggregate, filter, and prepare data for reporting and analytics.
• Steps:
1. Read data from the Silver tables.
2. Performed specific aggregations and calculations, such as total profit by 
product category, and total orders by state.
3. Saved the result datasets as Delta tables optimized for reporting.
6.2 Methodology
• Aggregation and Calculations:
o Total Profit by Product Category: Used grouping and aggregation 
functions to calculate total profits.
o Top 5 Customers by Spend: Identified the top-spending customers using 
ordering and limiting functions.
• Gold Table Structure: Saved the aggregated data in the following paths:
o /gold/amount_spent_per_city
o /gold/avg_profit_per_category_
o gold/avg_profit_per_city
o /gold/customer_amount--
o gold/customer_top5
o gold/product_orders
o gold/product_profit
o gold/product_revenue
o gold/state_orders
o gold/total_orders_per_category
7. Data Validation and Auditing
7.1 Task Overview
• Objective: Implement validation and auditing mechanisms to ensure data 
integrity across the ETL pipeline.
• Steps:
1. Implemented validation checks at each layer (Bronze, Silver, Gold).
2. Created timestamp at each layer to implement data lineage.
10
3. Created audit logs to track data lineage and changes.
4. Generated reports on data quality and transformation steps.
7.2 Methodology
• Validation Checks: Ensured data consistency by validating data types, row 
counts, and key constraints across layers.
• Audit Logs: Used Delta Lake's built-in features and current_timestamp to track 
changes and data lineage.
• Reporting: Generated summary reports on the quality of the data, the 
transformations applied, and any anomalies detected.
8. Final Output, Reporting, and Dashboarding
8.1 Task Overview
• Objective: Generate insights and visualizations using the Gold layer data.
• Steps:
1. Created visualizations using Databricks' built-in visualization tools.
2. Built a dashboard to display key metrics, such as total revenue and top 
customers.
3. Prepared a final report documenting the ETL pipeline, data 
transformations, and insights.
8.2 Methodology
• Visualization Tools: Utilized Databricks' built-in visualization tools to create 
insightful charts and graphs.
• Dashboard: Displayed key business metrics such as total sales, average order 
value, and customer lifetime value.
8.3 Dashboard-
11
o /gold/amount_spent_per_city
o /gold/avg_profit_per_category_
o gold/avg_profit_per_city
12
o /gold/customer_amount--
o gold/customer_top5
o gold/product_orders
o gold/product_profit
13
o gold/product_revenue
o gold/state_orders
o gold/total_orders_per_category
9. Conclusions
9.1 Project Summary
This project successfully implemented an end-to-end ETL pipeline in Databricks, from 
raw data ingestion to the creation of a comprehensive reporting layer. The pipeline 
maintained data integrity through rigorous quality checks and provided valuable 
business insights through data aggregation and reporting.
9.2 Future Work
14
Potential improvements could include:
• Automating the pipeline using azure data factory tools for regular data updates.
• Creating pipeline for the streaming data to regularly updating the database.
• Using of Unity catalog to store data more securely .
• Integrating additional data sources for richer analytics.
• Implementing machine learning models for predictive insights based on the Gold 
layer data.
