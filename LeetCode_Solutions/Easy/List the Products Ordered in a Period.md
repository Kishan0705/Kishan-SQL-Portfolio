### List the Products Ordered in a Period

**Problem Statement:**  
Get the names of products that have at least 100 units ordered in February 2020 and their total amount.  
Return the result in any order.

---

**Example Input:**

Products Table  
| product_id | product_name          | product_category |
|------------|-----------------------|------------------|
| 1          | Leetcode Solutions    | Book             |
| 2          | Jewels of Stringology | Book             |
| 3          | HP                    | Laptop           |
| 4          | Lenovo                | Laptop           |
| 5          | Leetcode Kit          | T-shirt          |

Orders Table  
| product_id | order_date | unit |
|------------|------------|------|
| 1          | 2020-02-05 | 60   |
| 1          | 2020-02-10 | 70   |
| 2          | 2020-01-18 | 30   |
| 2          | 2020-02-11 | 80   |
| 3          | 2020-02-17 | 2    |
| 3          | 2020-02-24 | 3    |
| 4          | 2020-03-01 | 20   |
| 4          | 2020-03-04 | 30   |
| 4          | 2020-03-04 | 60   |
| 5          | 2020-02-25 | 50   |
| 5          | 2020-02-27 | 50   |
| 5          | 2020-03-01 | 50   |

---

**SQL Query (Using Date Range Filtering - Recommended):**
```sql
SELECT 
  p.product_name, 
  SUM(o.unit) AS unit
FROM Products p 
JOIN Orders o 
  ON p.product_id = o.product_id  
WHERE o.order_date >= '2020-02-01' AND o.order_date < '2020-03-01'
GROUP BY p.product_name
HAVING SUM(o.unit) >= 100;
```

---

### Alternatives to Filter by February 2020

Use these depending on your SQL dialect:

---

#### 1. `EXTRACT(YEAR FROM ...)` and `EXTRACT(MONTH FROM ...)` (PostgreSQL, etc.)
```sql
WHERE EXTRACT(YEAR FROM o.order_date) = 2020 
  AND EXTRACT(MONTH FROM o.order_date) = 2
```

---

#### 2. `DATE_TRUNC()` (PostgreSQL, Snowflake, etc.)
```sql
WHERE DATE_TRUNC('month', o.order_date) = DATE '2020-02-01'
```

---

#### 3. `TO_CHAR()` (PostgreSQL, Oracle)
```sql
WHERE TO_CHAR(o.order_date, 'YYYY-MM') = '2020-02'
```

Or:
```sql
WHERE TO_CHAR(o.order_date, 'YYYY') = '2020'
  AND TO_CHAR(o.order_date, 'MM') = '02'
```

---

#### 4. `YEAR()` and `MONTH()` (MySQL, SQL Server)
```sql
WHERE YEAR(o.order_date) = 2020 AND MONTH(o.order_date) = 2
```

---

#### ✅ Summary Table

| SQL Dialect       | Preferred Approach                          |
|-------------------|---------------------------------------------|
| PostgreSQL        | `EXTRACT`, `DATE_TRUNC`, `TO_CHAR`          |
| MySQL             | `YEAR()`, `MONTH()`, or Date Range          |
| SQL Server        | `YEAR()`, `MONTH()`                         |
| Oracle            | `TO_CHAR()`, `TRUNC()`                      |
| Generic/Portable  | Date Range Filtering                        |
