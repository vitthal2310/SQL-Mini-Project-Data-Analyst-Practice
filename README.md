# SQL Window Functions: ROW_NUMBER vs RANK vs DENSE_RANK

A simple guide explaining the differences between **ROW_NUMBER()**, **RANK()**, and **DENSE_RANK()** with examples.

---

## 📌 1. ROW_NUMBER()
Assigns a **unique sequential number** to each row.

### When to use:
- When you want **no duplicate ranks**.
- When you want to **pick top N rows** strictly.

### Example:
```sql
SELECT 
    customer_id,
    total_sale,
    ROW_NUMBER() OVER (ORDER BY total_sale DESC) AS rn
FROM sales;
```

---

## 📌 2. RANK()
Gives **same rank for ties**, but **skips rank numbers**.

### When to use:
- When ties should have the **same rank**, but gaps are acceptable.

### Example:
```sql
SELECT 
    customer_id,
    total_sale,
    RANK() OVER (ORDER BY total_sale DESC) AS rnk
FROM sales;
```

---

## 📌 3. DENSE_RANK()
Gives **same rank for ties**, but **does NOT skip** the next rank.

### When to use:
- When ties should have the same rank and **no gaps** should appear.

### Example:
```sql
SELECT 
    customer_id,
    total_sale,
    DENSE_RANK() OVER (ORDER BY total_sale DESC) AS drnk
FROM sales;
```

---

## 📊 Difference Summary

| Function      | Ties Allowed | Gaps in Rank | Best Use Case |
|---------------|-------------|--------------|----------------|
| ROW_NUMBER    | ❌ No        | ❌ No         | Unique ordering |
| RANK          | ✔ Yes       | ✔ Yes        | Competition ranking |
| DENSE_RANK    | ✔ Yes       | ❌ No         | Group ranking |

---

## 📂 Repository Purpose
This repo helps beginners understand window functions with examples.

---

## 🧑‍💻 Author
Created with the help of ChatGPT.

