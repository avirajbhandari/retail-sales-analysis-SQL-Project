# Retail Sales SQL Analysis Project

## Overview

This project analyzes retail sales transaction data using PostgreSQL. The goal of the project is to clean the dataset, explore customer and sales behavior, and answer business-focused questions that a retail company could use to improve decision-making.

I completed this project to strengthen my SQL skills and demonstrate my ability to work with real transactional data. The analysis focuses on sales performance, customer behavior, product categories, monthly trends, and order patterns by time of day.

This project is designed as part of my data analytics portfolio for roles in data analytics, data operations, business intelligence, and finance-related analytics.

---

## Business Objective

The main objective of this project is to use SQL to answer practical retail business questions, such as:

- Which product categories generate the most sales?
- Who are the highest-value customers?
- Which month performs best in each year?
- How do transactions vary by gender and category?
- What time of day receives the most orders?
- How can messy transactional data be cleaned before analysis?

---

## Dataset

The dataset contains retail sales transactions with customer, product, and sales information.

Key fields include:

- `transactions_id`
- `sale_date`
- `sale_time`
- `customer_id`
- `gender`
- `age`
- `category`
- `quantity`
- `price_per_unit`
- `cogs`
- `total_sale`

---

## Tools Used

- PostgreSQL
- pgAdmin
- SQL
- GitHub

---

## Database Schema

```sql
CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

---

## Project Workflow

### 1. Database Creation

Created a PostgreSQL database and structured table to store retail transaction data.

### 2. Data Cleaning

Checked for missing values across all important columns and removed incomplete records to make the dataset reliable for analysis.

### 3. Data Exploration

Explored the dataset by identifying:

- Total number of sales
- Total number of unique customers
- Available product categories

### 4. Business Analysis

Wrote SQL queries to answer business questions related to sales, customers, categories, and order timing.

---

## Data Cleaning

```sql
SELECT *
FROM retail_sales
WHERE
    transactions_id IS NULL OR
    sale_date IS NULL OR
    sale_time IS NULL OR
    customer_id IS NULL OR
    gender IS NULL OR
    age IS NULL OR
    category IS NULL OR
    quantity IS NULL OR
    price_per_unit IS NULL OR
    cogs IS NULL OR
    total_sale IS NULL;
```

```sql
DELETE FROM retail_sales
WHERE
    transactions_id IS NULL OR
    sale_date IS NULL OR
    sale_time IS NULL OR
    customer_id IS NULL OR
    gender IS NULL OR
    age IS NULL OR
    category IS NULL OR
    quantity IS NULL OR
    price_per_unit IS NULL OR
    cogs IS NULL OR
    total_sale IS NULL;
```

---

## Data Exploration

### Total Sales Records

```sql
SELECT COUNT(*) AS total_sales
FROM retail_sales;
```

### Total Unique Customers

```sql
SELECT COUNT(DISTINCT customer_id) AS total_customers
FROM retail_sales;
```

### Product Categories

```sql
SELECT DISTINCT category
FROM retail_sales;
```

---

## Business Questions and SQL Solutions

### 1. Retrieve all sales made on a specific date

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

**Business Purpose:**  
Helps analyze sales activity on a specific business day.

---

### 2. Find Clothing transactions with high quantity sold in November 2022

```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND sale_date BETWEEN '2022-11-01' AND '2022-11-30'
  AND quantity >= 4;
```

**Business Purpose:**  
Identifies strong Clothing category transactions during a specific month.

---

### 3. Calculate total sales for each product category

```sql
SELECT
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category
ORDER BY net_sale DESC;
```

**Business Purpose:**  
Shows which product categories generate the most revenue.

---

### 4. Find the average age of customers who purchased Beauty products

```sql
SELECT
    category,
    ROUND(AVG(age)) AS average_age
FROM retail_sales
WHERE category = 'Beauty'
GROUP BY category;
```

**Business Purpose:**  
Helps understand the customer profile for Beauty products.

---

### 5. Find all high-value transactions above 1000

```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

**Business Purpose:**  
Identifies large transactions that may represent high-value customers or premium purchases.

---

### 6. Find the number of transactions by gender and category

```sql
SELECT
    category,
    gender,
    COUNT(transactions_id) AS total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category, total_transactions DESC;
```

**Business Purpose:**  
Helps compare purchasing behavior across gender and product categories.

---

### 7. Find the best-selling month in each year

```sql
SELECT
    year,
    month,
    avg_sale
FROM
(
    SELECT
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER(
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS ranking
    FROM retail_sales
    GROUP BY 1, 2
) AS monthly_sales
WHERE ranking = 1;
```

**Business Purpose:**  
Identifies the strongest sales month for each year based on average sale value.

---

### 8. Find the top 5 customers by total sales

```sql
SELECT
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

**Business Purpose:**  
Highlights the highest-value customers for retention or loyalty strategies.

---

### 9. Find the number of unique customers by category

```sql
SELECT
    category,
    COUNT(DISTINCT customer_id) AS total_customers
FROM retail_sales
GROUP BY category
ORDER BY total_customers DESC;
```

**Business Purpose:**  
Shows which categories attract the widest customer base.

---

### 10. Create sales shifts and count orders by shift

```sql
WITH hourly_sales AS
(
    SELECT
        *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Night'
        END AS shift
    FROM retail_sales
)

SELECT
    shift,
    COUNT(transactions_id) AS total_orders
FROM hourly_sales
GROUP BY shift
ORDER BY total_orders DESC;
```

**Business Purpose:**  
Helps understand what time of day has the highest order activity.

---

## Key SQL Skills Demonstrated

This project demonstrates the following SQL skills:

- Database and table creation
- Data cleaning with `IS NULL`
- Filtering with `WHERE`
- Date filtering using `BETWEEN`
- Aggregation using `COUNT()`, `SUM()`, and `AVG()`
- Grouping with `GROUP BY`
- Sorting with `ORDER BY`
- Customer segmentation
- Category-level sales analysis
- Window functions using `RANK()`
- Time-based analysis using `EXTRACT()`
- Conditional logic using `CASE WHEN`
- Common Table Expressions using `WITH`

---

## Key Business Insights

Some of the main insights this project can help uncover include:

- Which product categories generate the highest revenue
- Which customer segments are most active
- Which customers contribute the most to total sales
- Which month performs best in each year
- Which time of day receives the highest number of orders
- How sales performance differs across product categories and gender

---

## What I Learned

This project helped me improve my ability to use SQL for practical business analysis. I learned how to clean transaction-level data, write queries that answer business questions, and use SQL functions to analyze customer behavior, product performance, and time-based sales patterns.

It also helped me understand the importance of writing clean, readable SQL that can be understood by both technical and non-technical stakeholders.

---

## How This Project Is Relevant

This project reflects the type of analysis that data analysts and business intelligence teams perform in real companies. Retail businesses rely on transaction-level data to understand revenue trends, customer behavior, product performance, and operational patterns.

By completing this project, I practiced turning raw sales data into insights that could support better business decisions.

---

## Repository Structure

```text
retail-sales-sql-analysis/
│
├── README.md
├── retail_project.sql
├── retail_sales.csv
└── LICENSE
```

---

## Conclusion

This Retail Sales SQL Analysis project gave me hands-on experience working with transactional data in PostgreSQL. It strengthened my SQL foundation and helped me practice business-focused data analysis.

The project demonstrates my ability to clean data, explore datasets, write analytical SQL queries, and communicate insights clearly.

---

## Author

**Abhyudaya Rajbhandari**

Economics and Mathematics graduate with interests in data analytics, finance, consulting, and business operations.

I am building projects like this to strengthen my technical foundation in SQL, data analysis, and business problem-solving.

---

## License

This project is licensed under the MIT License.
