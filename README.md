
# 🛍️ Retail Sales Analysis SQL Project

## 📌 Project Overview
This beginner-friendly project showcases SQL techniques used by data analysts to explore, clean, and analyze retail sales data. It walks through the setup of a retail database, performs exploratory data analysis (EDA), and answers real business questions using SQL queries.

> **Database Used**: `p1_retail_db`  
> **Skill Level**: Beginner  
> **Tools**: PostgreSQL 
---

## 🎯 Objectives

- 📂 Set up and populate a retail sales database
- 🧹 Clean the data by handling null/missing records
- 📊 Perform EDA using SQL
- 📈 Answer business-related questions through queries
- 🧠 Derive actionable insights

---

## 🧱 Project Structure

### 1️⃣ Database Setup

```sql
CREATE DATABASE p1_retail_db;

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

### 2️⃣ Data Exploration & Cleaning

- Total number of records  
- Unique customer count  
- Unique product categories  
- Null value checks and removal

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

DELETE FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL
  OR gender IS NULL OR age IS NULL OR category IS NULL
  OR quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

---

## 📈 Business Analysis & SQL Queries

Here are some of the questions answered in the project:

- 🗓️ **Sales made on a specific date**  
```sql
SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';
```

- 👕 **Clothing category transactions with quantity > 4 in Nov 2022**  
```sql
SELECT * FROM retail_sales 
WHERE category = 'Clothing' 
AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11' 
AND quantity > 4;
```

- 💰 **Total sales by category**  
```sql
SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales GROUP BY category;
```

- 💄 **Average age of customers who purchased from 'Beauty'**  
```sql
SELECT ROUND(AVG(age), 2) AS avg_age 
FROM retail_sales 
WHERE category = 'Beauty';
```

- 💸 **High-value transactions (> 1000)**  
```sql
SELECT * FROM retail_sales WHERE total_sale > 1000;
```

- 🧍 **Transactions by gender and category**  
```sql
SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

- 📆 **Best selling month each year (avg sale)**  
```sql
SELECT year, month, avg_sale FROM (
  SELECT EXTRACT(YEAR FROM sale_date) AS year,
         EXTRACT(MONTH FROM sale_date) AS month,
         AVG(total_sale) AS avg_sale,
         RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date)
                     ORDER BY AVG(total_sale) DESC) AS rank
  FROM retail_sales
  GROUP BY year, month
) AS t1
WHERE rank = 1;
```

- 🏆 **Top 5 customers by total sales**  
```sql
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

- 📚 **Unique customer count per category**  
```sql
SELECT category, COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
```

- ⏰ **Sales shift analysis (Morning, Afternoon, Evening)**  
```sql
WITH hourly_sale AS (
  SELECT *,
         CASE 
           WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
           WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
           ELSE 'Evening'
         END AS shift
  FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

---

## 🔍 Key Insights

- **Customer Demographics**: Sales span across age groups and genders.
- **High-Value Transactions**: Several purchases exceed 1000 in value.
- **Sales Trends**: Monthly analysis shows seasonal fluctuations.
- **Top Customers**: High spenders identified for loyalty programs or promotions.
- **Popular Categories**: Clothing and Beauty lead in both quantity and revenue.

---

## 📋 How to Use

1. **Clone the Repository**
2. **Set up the Database** using provided schema
3. **Run the SQL Queries** for analysis
4. **Explore or Expand** with your own custom queries!

---

## 🧠 Conclusion

This project helped me strengthen my SQL fundamentals through practical, business-oriented use cases. It’s a solid starting point for any aspiring Data Analyst or Business Intelligence professional.
