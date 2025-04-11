## Day 4: Most Recent Transaction
**Difficulty:** Medium

###  Problem
Given a table of customer sales in a retail store with columns `id`, `transaction_value`, and `created_at` representing the date and time for each transaction, write a query to get the **last transaction for each day**.

📌 **Output should include:**
- ID of the transaction
- Datetime of the transaction
- Transaction amount

**Order the transactions by datetime.**

---

###  Given Data
```sql
CREATE TABLE customer_sales (
    id INT PRIMARY KEY,
    transaction_value DECIMAL(10, 2),
    created_at DATETIME
);

INSERT INTO customer_sales (id, transaction_value, created_at)
VALUES
(1, 50.00, '2025-01-23 10:15:00'),
(2, 30.00, '2025-01-23 15:45:00'),
(3, 20.00, '2025-01-23 18:30:00'),
(4, 45.00, '2025-01-24 09:20:00'),
(5, 60.00, '2025-01-24 22:10:00'),
(6, 25.00, '2025-01-25 11:30:00'),
(7, 35.00, '2025-01-25 14:50:00'),
(8, 55.00, '2025-01-25 19:05:00');
```

---

###  My Solution
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

### 💡 Other Solution
```sql
WITH split_date AS (
  SELECT
    id,
    transaction_value AS value,
    DATE(created_at) AS date,
    MAX(TIME(created_at)) AS latest_time,
    created_at
  FROM customer_sales
  GROUP BY date
)

SELECT
  id,
  created_at,
  CAST(value AS FLOAT) AS transaction_value
FROM split_date
GROUP BY date;
```

---

### 📘 Notes
- **My Approach:** Used `ROW_NUMBER()` to rank each transaction per day, ordered by the timestamp in descending order, and filtered to get the top (i.e., last) record per day.
- **Other Approach:** Grouped by date to extract the max time per day, but this method might not always return the exact row if multiple transactions share the same max time. My solution is more robust.

---


