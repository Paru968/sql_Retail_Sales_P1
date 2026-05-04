# 🛒 Retail Sales Analysis (SQL Project)

## 📌 Project Overview

This project focuses on analyzing retail sales data to extract meaningful business insights.
The dataset contains transaction-level information such as sales amount, customer details, product category, and purchase timing.

The main objective is to perform **data cleaning, exploration, and analysis using SQL** to solve real-world business problems.

---

## 🎯 Objectives

* Analyze sales performance over time
* Identify top-performing categories and customers
* Understand sales distribution by time (Morning, Afternoon, Evening)
* Calculate key metrics like total sales, average sales, and order trends

---

## 🗂️ Dataset Schema

| Column Name     | Data Type | Description             |
| --------------- | --------- | ----------------------- |
| transactions_id | INT       | Unique transaction ID   |
| sale_date       | DATE      | Date of sale            |
| sale_time       | TIME      | Time of sale            |
| customer_id     | INT       | Customer ID             |
| gender          | VARCHAR   | Customer gender         |
| age             | INT       | Customer age            |
| category        | VARCHAR   | Product category        |
| quantity        | INT       | Quantity purchased      |
| price_per_unit  | FLOAT     | Price per unit          |
| cogs            | FLOAT     | Cost of goods sold      |
| total_sale      | FLOAT     | Total transaction value |

---

## ⚙️ Table Creation

```sql
DROP TABLE IF EXISTS retail_sales111;

CREATE TABLE retail_sales111 (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(10),
    age INT,
    category VARCHAR(50),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

---

## 🧹 Data Cleaning

### Check NULL values

```sql
SELECT *
FROM retail_sales111
WHERE transactions_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR category IS NULL
   OR quantity IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```

### Remove NULL records

```sql
DELETE FROM retail_sales111
WHERE transactions_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR category IS NULL
   OR quantity IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```

---

## 📊 Data Exploration

```sql
-- Total records
SELECT COUNT(*) AS total_sales FROM retail_sales111;

-- Total customers
SELECT COUNT(customer_id) AS total_customers FROM retail_sales111;

-- Unique customers
SELECT COUNT(DISTINCT customer_id) AS unique_customers FROM retail_sales111;

-- Unique categories
SELECT DISTINCT category FROM retail_sales111;
```

---

## 🔍 Business Analysis Queries

### Q1: Sales on specific date

```sql
SELECT *
FROM retail_sales111
WHERE sale_date = '2022-11-05';
```

---

### Q2: Clothing sales (Nov 2022, quantity > 4)

```sql
SELECT *
FROM retail_sales111
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
```

---

### Q3: Total sales by category

```sql
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales111
GROUP BY category;
```

---

### Q4: Average age of Beauty category customers

```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales111
WHERE category = 'Beauty';
```

---

### Q5: Transactions with sales > 1000

```sql
SELECT *
FROM retail_sales111
WHERE total_sale > 1000;
```

---

### Q6: Transactions by gender and category

```sql
SELECT 
    category,
    gender,
    COUNT(*) AS total_transactions
FROM retail_sales111
GROUP BY category, gender;
```

---

### Q7: Monthly average sales

```sql
SELECT
    EXTRACT(YEAR FROM sale_date) AS year,
    EXTRACT(MONTH FROM sale_date) AS month,
    AVG(total_sale) AS avg_sale
FROM retail_sales111
GROUP BY year, month
ORDER BY year, month;
```

---

### Q8: Top 5 customers by total sales

```sql
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales111
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

---

### Q9: Unique customers per category

```sql
SELECT 
    category,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales111
GROUP BY category;
```

---

### Q10: Sales by shift (Morning / Afternoon / Evening)

```sql
WITH hourly_sales AS (
    SELECT 
        *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales111
)

SELECT 
    shift,
    COUNT(*) AS total_orders
FROM hourly_sales
GROUP BY shift
ORDER BY shift;
```

---

## 📈 Key Insights

* Afternoon sales show higher transaction volume
* Certain categories contribute significantly to total revenue
* High-value customers can be identified for targeted marketing

---

## 🚀 Conclusion

This project demonstrates how SQL can be used to clean, analyze, and extract insights from retail sales data for business decision-making.

---

## 🔮 Future Enhancements

* Build Power BI dashboard
* Add trend forecasting
* Automate ETL pipeline

---
