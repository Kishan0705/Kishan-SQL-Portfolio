
---

# Day 9 of 15: The PrepVector SQL Challenge  
**Approach Used:** `CASE`, `SUM()`, `GROUP BY`, `Pivoting with CASE`

---

## 📌 Problem Statement:

Write a query to find the total amount of each product sold for each month, with each product as its own column in the output table.

---

## 🧠 Thought Process:

Getting the total amount for each product is a straightforward process.  

However, since the requirement is to have a separate column for each product,  

we need to use a `CASE` statement and then `SUM` the results accordingly.

---

## ✅ SQL Query Solution:
```sql
SELECT
    month,
    SUM(CASE WHEN product_id = 1 THEN amount_sold ELSE 0 END) AS product_1,
    SUM(CASE WHEN product_id = 2 THEN amount_sold ELSE 0 END) AS product_2,
    SUM(CASE WHEN product_id = 3 THEN amount_sold ELSE 0 END) AS product_3,
    SUM(CASE WHEN product_id = 4 THEN amount_sold ELSE 0 END) AS product_4
FROM monthly_sales
GROUP BY month
ORDER BY month;
```

---

## 🔑 Key Takeaway:

• Using `CASE` statements allows us to pivot product data into separate columns.  
• `SUM(CASE WHEN...)` helps in calculating totals based on conditions.

---

