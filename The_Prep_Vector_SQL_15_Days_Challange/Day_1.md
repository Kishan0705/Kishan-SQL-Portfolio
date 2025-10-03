
---

# Day 1 of 15: The PrepVector SQL Challenge  
**Approach Used:** `CTE`, `CASE`, `CROSS JOIN`, `NOT IN`

---

## 📌 Problem Statement

You are given two tables: **`users`** and **`events`**.

- The **`users`** table contains a list of all users.  
- The **`events`** table records user activities, with an `action` column that can have one of three values:  
  - `'like'`  
  - `'comment'`  
  - `'post'`  

### **Task**  
Write an SQL query to calculate the **percentage of users who have never performed a 'like' or 'comment' action**.  
The result should be **rounded to two decimal places**.

---

## 🧠 Thought Process

### **1️⃣ First Approach**
- Count all distinct users who have never liked or commented using `NOT IN`.
- Retrieve the total user count separately.
- Calculate the percentage by dividing the two results.
- **Issue:** This approach is straightforward but required extra computation, making execution longer.

---

### **2️⃣ Optimized Approach**
- Use a `CASE` statement inside `COUNT()` to directly filter users who never liked or commented.
- Eliminates the need for extra joins and improves efficiency.

---

## ✅ SQL Query Solutions

### **Approach 1: Using CTE**
```sql
WITH never_liked_or_commented AS (
    SELECT
        COUNT(DISTINCT user_id) AS cnt
    FROM users
    WHERE user_id NOT IN (
        SELECT DISTINCT user_id
        FROM events
        WHERE action IN ('like', 'comment')
    )
),
total_users AS (
    SELECT COUNT(*) AS all_users FROM users
)
SELECT
    ROUND((n.cnt * 100.0) / t.all_users, 2) AS pct
FROM never_liked_or_commented n
CROSS JOIN total_users t;
```

---

### **Approach 2: Using CASE**
```sql
SELECT
    ROUND(
        (COUNT(CASE WHEN user_id NOT IN (
            SELECT DISTINCT user_id FROM events WHERE action IN ('like', 'comment')
        ) THEN 1 END) * 100.0) / COUNT(*), 2
    ) AS pct
FROM users;
```

---

### ✅ Key Takeaways
- **Approach 1** is more readable but involves multiple steps and joins.
- **Approach 2** is more efficient and concise for large datasets.

---

