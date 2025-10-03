
---

# Day 2 of 15: The PrepVector SQL Challenge  
**Approach Used:** `INNER JOIN`, `CASE`, `COUNT`, `ROUND`

---

## 📌 Problem Statement

You are given two tables: **`transactions`** and **`users`**.  
- Each transaction includes a **shipping address**.  
- Each user has a **primary home address** recorded in the `users` table.  

### **Task**  
Write a query to calculate the **percentage of transactions that were shipped to a user’s home address** rather than any other address.  
Return the result as **`home_address_percent`**, rounded to **two decimal places**.

---

## 🧠 Thought Process
- The `transactions` table contains both home and other shipping addresses.  
- To determine how often users order to their home address:  
  1. Join `transactions` with `users` on `user_id` to match each transaction with the user’s home address.  
  2. Use `CASE` inside `COUNT()` to count only those transactions where `shipping_address = home address`.  
  3. Calculate percentage:  
     (home address orders / total orders) * 100}

---

## ✅ SQL Query Solution
```sql
SELECT
    ROUND(
        (COUNT(CASE WHEN t.shipping_address = u.address THEN 1 END) * 100.0)
        / COUNT(*),
        2
    ) AS home_address_percent
FROM transactions t
JOIN users u ON t.user_id = u.id;
```

---

### 🔑 Key Takeaways
- **INNER JOIN:** Ensures only valid user transactions are considered.  
- **CASE with COUNT:** Filters and counts transactions shipped to the home address.  
- **Percentage Calculation:** `(home address orders / total orders) * 100`.  

---


