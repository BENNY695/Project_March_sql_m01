# Project_March_sql_m01 (Retail Sales Analysis)

## PROJECT OVERVIEW

**Project Title**: Retail Sales Analysis
**Level**: Beginner
**Database**: march_sql_m1

This project showcases SQL skills and techniques commonly used by data analysts to explore, clean, and analyze retail sales data. It includes creating a retail sales database, conducting exploratory data analysis (EDA), and using SQL queries to answer specific business questions. The project is well suited for beginners in data analysis who want to develop a strong foundation in SQL.

## OBJECTIVES

Database Setup: Create and populate a retail sales database using the provided sales data.
Data Cleaning: Detect and remove records that contain missing or null values.
Exploratory Data Analysis (EDA): Conduct basic analysis to gain an initial understanding of the dataset.
Business Analysis: Apply SQL queries to address key business questions and extract insights from the sales data.

## PROJECT STRUCTURE

### 1. DATABASE SETUP

- **Database Creation**: The project begins with the creation of a database called p1_retail_db.
- **Table Creation**: A table named retail_sales is then created to store the data. Its structure includes fields such as transaction ID, sale date and time, customer ID, gender, age, product category, quantity sold, unit price, cost of goods sold (COGS), and total sales amount.

"""sql
CREATE DATABASE march_sql_m1;
USE march_sql_m1;

CREATE TABLE retail_sales (
    transaction_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit DECIMAL(10,2),
    cogs DECIMAL(10,2),
    total_sale DECIMAL(10,2)
);
"""

### 2. DATA EXPLORATION & CLEANING 

- **Record Count**: Calculate the total number of records in the dataset.
- **Customer Count**: Determine the number of unique customers represented.
- **Category Count**: List all distinct product categories included in the data.
- **Null Value Check**: Identify any null or missing values and remove incomplete records.

""" sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
""" 

### 3. DATA ANALYSIS & FINDINGS 

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
"""sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
'''

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
"""sql  
SELECT *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4;
""" 

3. **Write a SQL query to calculate the total sales (total_sale) for each category**:
"""sql
 SELECT
     category,
     SUM(total_sale) as net_sale,
     COUNT(*) as total_orders
 FROM retail_sales
 GROUP BY 1;
""" 

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category**:
´´´sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty';
""" 

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000**:
"""sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
""" 

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category**:
"""  
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1;
""" 

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
"""sql  
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1;
""" 

8. **Write a SQL query to find the top 5 customers based on the highest total sales**:
"""sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
""" 

9. **Write a SQL query to find the number of unique customers who purchased items from each category**:
"""sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category;
""" 

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
"""sql  
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift;
"""

## FINDINGS

Customer Demographics: The dataset covers a wide range of age groups, with sales spread across categories such as Clothing and Beauty.
High-Value Transactions: Multiple transactions exceed a total sale value of 100 hookup 1,000, suggesting the presence of premium purchases.
Sales Trends: Monthly sales analysis reveals fluctuations over time, making it easier to identify peak periods.
Customer Insights: The analysis highlights the highest-spending customers and the most popular product categories.

## REPORTS

Sales Summary: A comprehensive report outlining total sales, customer demographics, and performance by product category.
Trend Analysis: Insights highlighting sales patterns across different months and time periods.
Customer Insights: Reports identifying top customers and showing the number of unique customers within each category.

## CONCLUSION

This project provides a thorough introduction to using SQL in data analysis, encompassing database creation, data cleaning, exploratory analysis, and business-focused querying. The insights generated can support informed decision-making by revealing sales trends, customer behavior, and product performance.

## How to Use

Clone the Repository: Download a copy of this project from GitHub to your local machine.
Set Up the Database: Execute the SQL scripts in the database_setup.sql file to create and populate the database.
Run the Queries: Run the SQL statements in analysis_queries.sql to carry out the analysis.
Explore and Modify: You can adjust the queries to investigate other aspects of the dataset or answer additional business questions.
