### Group Sold Product By The Date

**Write a solution to find for each date the number of different products sold and their names.**  
The sold products names for each date should be sorted lexicographically.

Return the result table ordered by `sell_date`.

---

**Input:**

```sql
Activities table:
+------------+------------+
| sell_date  | product     |
+------------+------------+
| 2020-05-30 | Headphone  |
| 2020-06-01 | Pencil     |
| 2020-06-02 | Mask       |
| 2020-05-30 | Basketball |
| 2020-06-01 | Bible      |
| 2020-06-02 | Mask       |
| 2020-05-30 | T-Shirt    |
+------------+------------+
```

---

**Query (MySQL):**

```sql
SELECT
  sell_date, 
  COUNT(DISTINCT product) AS num_sold,
  GROUP_CONCAT(DISTINCT product ORDER BY product ASC SEPARATOR ',') AS products
FROM Activities 
GROUP BY sell_date 
ORDER BY sell_date ASC;
```

---
```sql

Output: 
+------------+----------+------------------------------+
| sell_date  | num_sold | products                     |
+------------+----------+------------------------------+
| 2020-05-30 | 3        | Basketball,Headphone,T-shirt |
| 2020-06-01 | 2        | Bible,Pencil                 |
| 2020-06-02 | 1        | Mask                         |
+------------+----------+------------------------------+
```

**PostgreSQL Equivalent:**

```sql
SELECT
  sell_date, 
  COUNT(DISTINCT product) AS num_sold,
  STRING_AGG(DISTINCT product, ',' ORDER BY product) AS products
FROM Activities 
GROUP BY sell_date 
ORDER BY sell_date ASC;
```
