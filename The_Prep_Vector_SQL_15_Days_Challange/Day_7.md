
---

# Day 7 of 15: The PrepVector SQL Challenge  
**Approach Used:** `CTE`, `COUNT(DISTINCT)`, `HAVING`, `GROUP BY`, `Subquery`

---

## 📌 Problem Statement:

Write a query to get the number of customers that were upsold by purchasing additional products.

---

## 🧠 Thought Process:

• First, I grouped the transactions by user id.  

• Then, I used `COUNT(DISTINCT DATE(created_at))` to determine the number of unique days each customer made a purchase.  

• Next, I applied a `HAVING` clause to filter only those customers who made purchases on more than one distinct date, ensuring that same-day purchases don’t count as upsells.  

• Finally, used this result in a subquery and used `COUNT(user_id)` to get the total number of customers who were upsold.

---

## ✅ SQL Query Solutions:

### **CTE Approach**
```sql
WITH cte AS (
    SELECT
        user_id,
        COUNT(DISTINCT DATE(created_at)) AS purchase_days
    FROM transactions
    GROUP BY user_id
)
SELECT
    COUNT(user_id) AS upsold_customer_count
FROM cte
WHERE purchase_days > 1;
```

---

### **Subquery Approach**
```sql
SELECT
    COUNT(user_id) AS upsold_customers
FROM (
    SELECT
        user_id
    FROM transactions
    GROUP BY user_id
    HAVING COUNT(DISTINCT DATE(created_at)) > 1
) AS upsold_users;
```

---

## 🔑 Key Takeaway:

Identifying upsold customers helps analyze purchasing behavior, optimize sales strategies, and understand customer retention by tracking purchases across multiple days.

---
