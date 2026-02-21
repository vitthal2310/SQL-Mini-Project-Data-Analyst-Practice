# SQL-Mini-Project – Data Analyst Practice

 Project Overview

This SQL project demonstrates end-to-end data analysis using five connected datasets:

retail_sales,
customers,
orders,
employees,
website_traffic

What You Will Practice

Creating tables
Inserting sample data
Data validation queries
15 real-world SQL analytical tasks, including:

Joins (INNER, LEFT),
Window functions (ROW_NUMBER, RANK, DENSE_RANK),
Aggregations (SUM, AVG, COUNT),
Date functions (EXTRACT, DATE_TRUNC),
Conditional logic (CASE),
Grouping & subqueries

## 🎯 Objectives

✔ Create & populate relational tables
✔ Perform real data analysis
✔ Solve 15 business SQL questions
✔ Demonstrate analyst-level SQL mastery

## 1. Database Schema Setup
## Create Tables
```sql
DROP TABLE IF EXISTS retail_sales;
CREATE TABLE retail_sales (
    sale_id INT PRIMARY KEY,
    customer_id VARCHAR(10),
    product VARCHAR(20),
    total_sale INT,
    sale_date DATE,
    sale_time TIME
);

CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(20),
    city VARCHAR(20)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_amount INT,
    order_date DATE
);

CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(20),
    dept VARCHAR(20),
    salary INT,
    join_date DATE
);

DROP TABLE IF EXISTS website_traffic;
CREATE TABLE website_traffic (
    visit_id INT PRIMARY KEY,
    user_id INT,
    visit_date DATE,
    session_duration_sec INT
);
```

## 2. Insert Sample Data
Retail Sales
```sql
INSERT INTO retail_sales (sale_id, customer_id, product, total_sale, sale_date, sale_time)
VALUES
(1, 'C101', 'Shirt', 1200, '2024-01-01', '09:20:00'),
(2, 'C102', 'Shoes', 2500, '2024-01-01', '15:10:00'),
(3, 'C103', 'Laptop', 55000, '2024-01-02', '18:00:00'),
(4, 'C101', 'Shoes', 2500, '2024-01-03', '11:10:00'),
(5, 'C104', 'Bag', 1800, '2024-01-04', '20:30:00');

```
Customers
```sql
ALTER TABLE customers ALTER COLUMN customer_id TYPE VARCHAR(10);

INSERT INTO customers (customer_id, name, city)
VALUES
('C101', 'Amit', 'Pune'),
('C102', 'Rohit', 'Mumbai'),
('C103', 'Sneha', 'Delhi'),
('C104', 'Priya', 'Pune');

```
Orders
```sql
ALTER TABLE orders RENAME COLUMN ustomer_id TO customer_id;
ALTER TABLE orders ALTER COLUMN customer_id TYPE VARCHAR(10);

INSERT INTO orders (order_id, customer_id, order_amount, order_date)
VALUES
(1, 'C101', 1200, '2024-01-01'),
(2, 'C102', 5000, '2024-01-03'),
(3, 'C101', 1500, '2024-01-10'),
(4, 'C104', 900, '2024-01-12'),
(5, 'C102', 7000, '2024-01-15');

```

Employees
```sql
INSERT INTO employees (emp_id, name, dept, salary, join_date)
VALUES
(1, 'Asha', 'HR', 40000, '2022-01-01'),
(2, 'Arjun', 'Finance', 55000, '2021-03-05'),
(3, 'Neha', 'IT', 80000, '2020-10-10'),
(4, 'Karan', 'Finance', 60000, '2023-06-01');

```

Website Traffic
```sql
ALTER TABLE website_traffic ALTER COLUMN user_id TYPE VARCHAR(10);

INSERT INTO website_traffic (visit_id, user_id, visit_date, session_duration_sec)
VALUES
(1, 'U1', '2024-01-01', 300),
(2, 'U2', '2024-01-01', 120),
(3, 'U1', '2024-01-02', 250),
(4, 'U3', '2024-01-02', 500);

```

## 3. Validate Inserted Data

```sql
SELECT * FROM retail_sales;
SELECT * FROM customers;
SELECT * FROM orders;
SELECT * FROM employees;
SELECT * FROM website_traffic;

```

## 4. Data Analyst SQL Tasks (15 Queries)
## 1. Total sales per day
```sql
SELECT sale_date, SUM(total_sale) AS total_sales_per_day
FROM retail_sales
GROUP BY sale_date
ORDER BY sale_date;

```

## 2. Top 3 highest selling products
```sql
SELECT product, SUM(total_sale) AS total_sale
FROM retail_sales
GROUP BY product
ORDER BY total_sale DESC
LIMIT 3;

```

## 3. Identify time-based order category
```sql
SELECT *,
       CASE
           WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'morning'
           WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'afternoon'
           ELSE 'evening'
       END AS shift
FROM retail_sales;

```

## 4. Order count per customer
```sql
SELECT customer_id, COUNT(*) AS total_orders
FROM retail_sales
GROUP BY customer_id;

```

## 5. Highest sales day per month
```sql
SELECT *
FROM (
        SELECT
            EXTRACT(YEAR FROM sale_date) AS year,
            EXTRACT(MONTH FROM sale_date) AS month,
            sale_date,
            SUM(total_sale) AS sale_per_day,
            RANK() OVER (
                PARTITION BY EXTRACT(YEAR FROM sale_date), EXTRACT(MONTH FROM sale_date)
                ORDER BY SUM(total_sale) DESC
            ) AS rnk
        FROM retail_sales
        GROUP BY sale_date
     ) t
WHERE rnk = 1;

```

## 6. Customers with no purchase
```sql
SELECT c.customer_id, c.name
FROM customers c
LEFT JOIN retail_sales r
    ON c.customer_id = r.customer_id
WHERE r.customer_id IS NULL;

```

## 7. Customer count by city
```sql
SELECT city, COUNT(*) AS customer_count
FROM customers
GROUP BY city;

```

## 8. Customers who purchased more than 3 times
```sql
SELECT
      r.customer_id,
      COUNT(*) AS purchase_count
FROM retail_sales r
JOIN customers c ON r.customer_id = c.customer_id
GROUP BY r.customer_id
HAVING COUNT(*) > 3;

```

## 9. Monthly total order amount
```sql
SELECT
      EXTRACT(YEAR FROM order_date) AS year,
      EXTRACT(MONTH FROM order_date) AS month,
      SUM(order_amount) AS monthly_total
FROM orders
GROUP BY 1, 2
ORDER BY 1, 2;

```

## 10. Customers with total spending > 5000
```sql
SELECT
      c.customer_id,
      SUM(o.order_amount) AS total_spending
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id
HAVING SUM(o.order_amount) > 5000;

```

## 11. 2nd highest spending customer
```sql
SELECT customer_id, total_spending
FROM (
        SELECT
            c.customer_id,
            SUM(o.order_amount) AS total_spending,
            DENSE_RANK() OVER (ORDER BY SUM(o.order_amount) DESC) AS rnk
        FROM customers c
        JOIN orders o ON c.customer_id = o.customer_id
        GROUP BY c.customer_id
     ) t
WHERE rnk = 2;

```

## 12. Average salary by department
```sql
SELECT dept, ROUND(AVG(salary), 1) AS avg_salary
FROM employees
GROUP BY dept;

```
## 13. Employees who joined after 2022
```sql
SELECT *
FROM employees
WHERE EXTRACT(YEAR FROM join_date) > 2022;

```

## 14. Highest salary per department
```sql
SELECT dept, salary
FROM (
        SELECT
            dept,
            salary,
            ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC) AS rn
        FROM employees
     ) t
WHERE rn = 1;

```

## 15. Daily average session duration
```sql
SELECT
    visit_date,
    EXTRACT(YEAR FROM visit_date) AS year,
    EXTRACT(MONTH FROM visit_date) AS month,
    EXTRACT(DAY FROM visit_date) AS day,
    ROUND(AVG(session_duration_sec), 2) AS avg_session_duration
FROM website_traffic
GROUP BY visit_date
ORDER BY visit_date;

```

## Conclusion

This SQL mini-project demonstrates:

Joins
Aggregations
Ranking functions
Window functions
CASE expressions
Date/time extraction
Subqueries

## Author

Vitthal Bidve

