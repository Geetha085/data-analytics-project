# 🛍️ Retail Sales Analysis SQL Project

## 📌 Project Overview

**Project Title:** Retail Sales Analysis 
**Database Used:** `sql_project`  
**Tool:** MySQL Workbench  

This project demonstrates the use of SQL for real-world data analysis. It includes setting up a retail sales database, cleaning and exploring data, and answering key business questions using SQL queries. It’s ideal for beginners who want to build confidence in writing SQL queries for data analysis.

---

## 🎯 Objectives

1. **Set up a retail sales database** with sales transaction data.
2. **Clean the data** by identifying and removing null or incomplete records.
3. **Explore the dataset** using SQL to understand key patterns.
4. **Answer business questions** through data analysis.

---

## 🗂️ Project Structure

### 1. 🏗️ Database Setup

- **Database Name:** `sql_project`
- **Main Table:** `retail_sales`

```sql
CREATE DATABASE IF NOT EXISTS sql_project;
USE sql_project;

CREATE TABLE retail_sales (
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
```

---

### 2. 🧹 Data Exploration & Cleaning

```sql
-- Record count
SELECT COUNT(*) FROM retail_sales;

-- Unique customers
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- Unique product categories
SELECT DISTINCT category FROM retail_sales;

-- Check for nulls
SELECT * FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR
      gender IS NULL OR age IS NULL OR category IS NULL OR
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Remove null records
DELETE FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR
      gender IS NULL OR age IS NULL OR category IS NULL OR
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

---

## Business Analysis Queries

### 1. Write a SQL query to retrieve all columns for sales made on '2022-11-05:
```sql
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
```

### 2. Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022:
```sql
SELECT * FROM retail_sales
WHERE category = 'Clothing'
  AND DATE_FORMAT(sale_date, '%Y-%m') = '2022-11'
  AND quantity > 4;
```

### 3. Write a SQL query to calculate the total sales (total_sale) for each category.:
```sql
SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

### 4. Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:
```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

### 5. Write a SQL query to find all transactions where the total_sale is greater than 1000.:
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000;
```

### 6. Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.:
```sql
SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

### 7. Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:
```sql
SELECT year, month, avg_sale
FROM (
    SELECT 
        YEAR(sale_date) AS year,
        MONTH(sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (PARTITION BY YEAR(sale_date) ORDER BY AVG(total_sale) DESC) AS rnk
    FROM retail_sales
    GROUP BY year, month
) AS ranked
WHERE rnk = 1;
```

### 8. Write a SQL query to find the top 5 customers based on the highest total sales 
```sql
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

### 9. Write a SQL query to find the number of unique customers who purchased items from each category.:
```sql
SELECT category, COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
```

### 10. Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):
```sql
SELECT 
    CASE
        WHEN HOUR(sale_time) < 12 THEN 'Morning'
        WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END AS shift,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY shift;
```

---

## Findings

- **Customer Demographics:** The dataset includes customers from various age groups, buying across diverse product categories.
- **High-Value Transactions:** Some sales exceeded ₹1000, indicating premium items or large orders.
- **Seasonality & Trends:** Certain months show higher average sales, helping identify peak business periods.
- **Customer Behavior:** Identified top customers and categories with high engagement and spending.
