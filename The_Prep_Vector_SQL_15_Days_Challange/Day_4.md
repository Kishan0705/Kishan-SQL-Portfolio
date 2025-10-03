
---

# Day 4 of 15: The PrepVector SQL Challenge  
**Approach Used:** `ROW_NUMBER()`, `LAST_VALUE()`, `CTE`, `LAST_VALUE & Frame Clause`

---

## 📌 Problem Statement  
In the **Customer Sales** table, we have columns such as `id`, `transaction_amount`, and `created_at`.  
We need to **find the last transaction of each day**.

---

## 🧠 Thought Process  

### **1️⃣ First Approach: Using ROW_NUMBER()**
- Assign a row number to each transaction **within each day**, ordered by `created_at DESC`.
- Filter for `rn = 1` to get the latest transaction per day.

---

### **2️⃣ Second Approach: Using LAST_VALUE()**
- Use `LAST_VALUE()` to directly retrieve the last transaction per day.
- **Important:** Specify the **frame clause** because the default frame (`RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`) can lead to incorrect results.

---

## ✅ SQL Query Solutions  

### **Approach 1: Using ROW_NUMBER()**
```sql
WITH CTE AS (
    SELECT
        *,
        ROW_NUMBER() OVER (PARTITION BY DATE(created_at) ORDER BY created_at DESC) AS rnk
    FROM customer_sales
)
SELECT
    id,
    transaction_value,
    created_at
FROM CTE
WHERE rnk = 1
ORDER BY created_at;
```

---

### **Approach 2: Using LAST_VALUE()**
```sql
WITH CTE AS (
    SELECT
        *,
        LAST_VALUE(id) OVER (
            PARTITION BY DATE(created_at)
            ORDER BY created_at
            RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
        ) AS last_id
    FROM customer_sales
)
SELECT
    id,
    transaction_value,
    created_at
FROM CTE
WHERE id = last_id
ORDER BY created_at;
```

---

### 🔑 Key Takeaways  
✔️ **Window Functions** are powerful for ranking and aggregation problems.  
✔️ When using **LAST_VALUE()**, always define the **frame clause** to avoid incorrect results.  
✔️ `ROW_NUMBER()` is often simpler and more intuitive for “latest record” problems.  

---


