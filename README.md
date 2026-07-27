# Sql_Retail_Sale_P1
A comprehensive MySQL project covering database setup, data cleaning, exploratory analysis, and business-driven SQL queries.
-- Data Analysis & Business Key Problems & Answers

-- My Analysis & Findings
-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05
-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022
-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
-- Q.8 Write a SQL query to find the top 5 customaers based on the highest total sales 
-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)

## Project Overview

-- Project Title: Retail Sales Analysis  
-- Level: Beginner  
-- Database: `sql_retail_sales_analysis.db`
## Objectives

-- Set up a retail sales database: Create and populate a retail sales database with the provided sales data.
-- Data Cleaning: Identify and remove any records with missing or null values.
-- Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.
-- Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.

SELECT * FROM retail_sales;

CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);

TRUNCATE `sql_retail_sales_analysis`.`retail_sales`;

-- 2. Data Exploration & Cleaning
 Record Count: Determine the total number of records in the dataset.

 SELECT COUNT(*) FROM retail_sales;
 ## Customer Count: Find out how many unique customers are in the dataset.
 SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
 ### Category Count: Identify all unique product categories in the dataset.
 SELECT COUNT(DISTINCT category)  FROM retail_sales;
 ## Null Value Check: Check for any null values in the dataset and delete records with missing data.


ALTER TABLE `sql_retail_sales_analysis`.`retail_sales`
CHANGE COLUMN `quantiy` `quantity` INT NULL DEFAULT NULL;

SELECT * FROM retail_sales;

SELECT * 
FROM retail_sales
WHERE transactions_id IS NULL
OR
WHERE sale_date IS NULL
OR
WHERE sale_time IS NULL
OR
WHERE customer_id IS NULL
OR
WHERE gender IS NULL
OR
WHERE age IS NULL
OR
WHERE cateqory IS NULL
OR
WHERE quantity IS NULL
OR
WHERE price_per_unit IS NULL
OR
WHERE cogs IS NULL
OR
WHERE total_sale IS NULL;

-- Delete Null Rows
DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Data Analysis & Findings
-- The following SQL queries were developed to answer specific business questions:
-- Data Analysis & Business Key Problems & Answers
-- My Analysis & Findings
-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';

SELECT COUNT(*)                                                                                 FROM retail_sales;
-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022
SELECT 
    *
FROM
    retail_sales
WHERE
    category = 'Clothing'
    AND
	sale_date BETWEEN "2022-11-01" AND "2022-11-30"
    AND 
    quantity < 10;
    

-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
SELECT 
category, 
SUM(total_sale) as net_sale,
COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1;
-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
SELECT * FROM retail_sales;

SELECT round(avg(age), 2) as avg_age FROM retail_sales
WHERE category = "Beauty";

-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
SELECT * FROM retail_sales
WHERE total_sale > 1000;

-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
SELECT category,gender as each_cat, COUNT(*) as total_sales, SUM(total_sale), ROUND(AVG(total_sale))
FROM retail_sales
group by category, gender;
-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
SELECT Year_, Month_, avg_sales FROM (
SELECT 
	EXTRACT(YEAR FROM sale_date) as Year_,
	EXTRACT(MONTH FROM sale_date) as Month_,
    ROUND(AVG(total_sale),2) as avg_sales,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as ranking
FROM
    retail_sales
GROUP BY 1, 2
-- ORDER BY 1, 2, 3 DESC;
) as t1
WHERE ranking = 1;
-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales

SELECT customer_id, SUM(total_sale) as total_sales FROM retail_sales
group by 1
ORDER BY 2 DESC;

-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.

SELECT category, COUNT(DISTINCT customer_id) as uni_customer FROM retail_sales
group by category;

-- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)
SELECT * FROM retail_sales;

WITH hourly_sale 
AS
(
SELECT *,
CASE
	WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Mornig'
    WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
    WHEN EXTRACT(HOUR FROM sale_time) < 17 THEN 'Afternoon'
    ELSE "Evening"
END as shift
FROM retail_sales
)
SELECT shift, COUNt(transactions_id) as id FROM hourly_sale
GROUP BY shift;

-- SELECT EXTRACT(HOUR FROM CURRENT_TIME);

-- End Of Project
## Findings

-- Customer Demographics: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
-- High-Value Transactions: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
-- Sales Trends: Monthly analysis shows variations in sales, helping identify peak seasons.
-- Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

-- Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
-- Trend Analysis**: Insights into sales trends across different months and shifts.
-- Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

-- This repository walks through a complete SQL workflow built for data analysts — starting from database setup, moving through data cleaning, and finishing with exploratory analysis and business-oriented queries. The idea is to go beyond basic syntax practice and demonstrate how SQL is actually used in real-world analytics work.The project is broken down into practical stages: preparing and structuring the database, handling messy or inconsistent data, exploring the dataset to understand its structure and patterns, and finally writing targeted queries to answer specific business questions. Along the way, the analysis touches on sales trends, customer behavior, and product performance — the kind of insights that can directly support decision-making.Whether you're learning SQL from scratch or looking to sharpen your query-writing skills for real analytical tasks, this project is meant to serve as a practical, applied reference.














