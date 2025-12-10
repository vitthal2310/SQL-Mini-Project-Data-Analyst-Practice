# SQL-Mini-Project – Data Analyst Practice

Retail + Customers + Orders + Employees + Web Traffic — SQL Analytics Project

📌 Project Overview

This SQL project demonstrates end-to-end data analysis using 5 connected datasets:

retail_sales, customers, orders, employees, website_traffic


# 🎯 Objectives

✔ Create & populate relational tables
✔ Perform real data analysis
✔ Solve 15 business SQL questions
✔ Demonstrate analyst-level SQL mastery

# 1. Database Schema Setup
# Drop & Create Tables
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

---

# 2. Insert Sample Data
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

# 3. Validate Inserted Data

```sql
SELECT * FROM retail_sales;
SELECT * FROM customers;
SELECT * FROM orders;
SELECT * FROM employees;
SELECT * FROM website_traffic;

```

# 4. Data Analyst SQL Tasks (15 Queries)
# 1. Total sales per day
```sql
SELECT sale_date, SUM(total_sale) AS total_sales_per_day
FROM retail_sales
GROUP BY sale_date
ORDER BY sale_date;

```

# 2. Top 3 highest selling products
```sql
SELECT product, SUM(total_sale) AS total_sale
FROM retail_sales
GROUP BY product
ORDER BY total_sale DESC
LIMIT 3;

```

tGPT.

