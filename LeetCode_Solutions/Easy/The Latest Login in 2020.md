### The Latest Login in 2020

Write a solution to report the **latest login** for all users **in the year 2020**.

- Do **not** include users who did not login in 2020.
- Return the result table in **any order**.

---

**Input:**

```sql
Logins table:
+---------+---------------------+
| user_id | time_stamp          |
+---------+---------------------+
| 6       | 2020-06-30 15:06:07 |
| 6       | 2021-04-21 14:06:06 |
| 6       | 2019-03-07 00:18:15 |
| 8       | 2020-02-01 05:10:53 |
| 8       | 2020-12-30 00:46:50 |
| 2       | 2020-01-16 02:49:50 |
| 2       | 2019-08-25 07:59:08 |
| 14      | 2019-07-14 09:00:00 |
| 14      | 2021-01-06 11:59:59 |
+---------+---------------------+
```

---

**Query (PostgreSQL version):**

```sql
SELECT 
  user_id, 
  MAX(time_stamp) AS last_stamp
FROM Logins 
WHERE TO_CHAR(time_stamp, 'YYYY') = '2020'
GROUP BY user_id;
```

---

### 🛠️ SQL Alternatives for Year Filtering

| SQL Dialect       | Expression                            | Notes                             |
|-------------------|----------------------------------------|------------------------------------|
| PostgreSQL        | `TO_CHAR(time_stamp, 'YYYY') = '2020'` | Works well for string comparison   |
| PostgreSQL        | `EXTRACT(YEAR FROM time_stamp) = 2020` | Numeric comparison                 |
| MySQL / SQL Server| `YEAR(time_stamp) = 2020`              | Use this for MySQL/SQL Server      |
| Generic / Index-Friendly | `time_stamp >= '2020-01-01' AND time_stamp < '2021-01-01'` | Best for performance  |

---
