Project Overview

This project focuses on analyzing retail sales data to extract meaningful business insights.
The dataset includes transaction-level details such as sales amount, customer information, product category, and time of purchase.

The main goal is to perform data cleaning, transformation, and analysis using SQL to answer key business questions.
**Objectives**
Analyze sales performance over time
Identify top-performing categories and customers
Understand sales distribution by time (Morning, Afternoon, Evening)
Calculate key metrics like total sales, average sales, and order trends

**--sql retail sales analysis-p1
**drop datle if exist retail_sales
CREATE TABLE retail_sales111 (
		    transactions_id INT PRIMARY KEY,
		    sale_date DATE,
		    sale_time TIME,
		    customer_id INT,
		    gender VARCHAR(10),
		    age INT,
		    category VARCHAR(50),
		    quantiy INT,
		    price_per_unit FLOAT,
		    cogs FLOAT,
		    total_sale FLOAT
);

--fetching null values
--data cleaning

select * from retail_sales111;

select * from retail_sales111
where transactions_id is null
or sale_date is null
or sale_time is null
or customer_id is null
or gender is null
or category is null
or quantiy is null 
or price_per_unit is null 
or cogs is null
or total_sale is null;


select * from retail_sales111;
--delete the records which haVING NULL VALUES
DELETE FROM retail_sales111


where transactions_id is null
or sale_date is null
or sale_time is null
or customer_id is null
or gender is null
or category is null
or quantiy is null 
or price_per_unit is null 
or cogs is null
or total_sale is null;

select 
count(*)
from retail_sales111;

--data exploration
--the business what see how many records we have in total sales
select count(*) as total_sales from retail_sales111;

--how many customers we have actually
select count (customer_id ) as total_customers from retail_sales111;
--how many unique customers
select count(distinct customer_id) as unique_customers from retail_sales111;

--ctegory as distuinict
select  distinct category  from retail_sales111;

--data analysis & business key probleams &ans

--q1. write a sql query to retrive all column for sales made on 2022-11-05
select *
from retail_sales111
where sale_date='2022-11-05';


--q2.q1. write a sql query to retrive all transactions where category is 'clothing'
--and quantity sold in more than 10 in month of nov-2022

select *
from retail_sales111
where category='Clothing'
and
To_char(sale_date,'yyyy-mm')='2022-11'
and
quantiy>=4;

--write a sql query to calculate total sales for each category
select category,
sum(total_sale) as net_sale,
count(*) as total_orders
from retail_sales111
group by 1;


--q4. write a sql query to fing avg of customers who parchased items from the beauty category
select round(avg(age),2) as total_age

from retail_sales111
where category='Beauty';

--q5.write a sql query to find all transcations where total sale >1000
select *
from retail_sales111
where total_sale>1000;

--q6. write a sql quey to find total number of transactions made by each gender in each category
select
category,
gender,
count(*) as total_trans
from retail_sales111
group by
	category,
	gender;
--q7. write a sql query to calculate avg sale for eac month fing best selling month in each year
SELECT
    EXTRACT(YEAR FROM sale_date) AS year,
    EXTRACT(MONTH FROM sale_date) AS month,
    AVG(total_sale) AS avg_sale
FROM RETAIL_SALES111
GROUP BY 1, 2
ORDER BY 1,2 DESC;

--Q8. WRITE A QUERY TO FIND TOP5 BCUSTOMERS BASED ON HEIGHT BSALARY

select customer_id,
sum(total_sale) as total_sales
from retail_sales111
group by 1
limit 5;


--q9. write a query to find number of unique customerws who purchased items from each cvategory
select
category,
count( distinct customer_id) as unique_customers
from retail_sales111
group by 1;

--q10write a sql query create each shiftg and number of orders
--ex(morning<=12 afternoon bw12 17, evening is 5 like that)
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

--END OF PROJECT
