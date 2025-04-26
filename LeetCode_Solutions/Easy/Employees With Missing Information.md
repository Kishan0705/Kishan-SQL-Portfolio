### Employees With Missing Information

Write a solution to report the IDs of all the employees with **missing information**.

An employee’s information is considered missing if:
- The employee's **name is missing**, or
- The employee's **salary is missing**

---

**Return** the result table **ordered by `employee_id` in ascending order**.

---

**Input:**

```sql
Employees table:
+-------------+----------+
| employee_id | name     |
+-------------+----------+
| 2           | Crew     |
| 4           | Haven    |
| 5           | Kristian |
+-------------+----------+

Salaries table:
+-------------+--------+
| employee_id | salary |
+-------------+--------+
| 5           | 76071  |
| 1           | 22517  |
| 4           | 63539  |
+-------------+--------+
```

---

**Query:**

```sql
SELECT e.employee_id
FROM Employees e
LEFT JOIN Salaries s ON e.employee_id = s.employee_id
WHERE s.salary IS NULL

UNION

SELECT s.employee_id
FROM Salaries s
LEFT JOIN Employees e ON s.employee_id = e.employee_id
WHERE e.name IS NULL
ORDER BY employee_id;
```

---

**Explanation:**

1. The first part of the query:
   ```sql
   SELECT e.employee_id
   FROM Employees e
   LEFT JOIN Salaries s ON e.employee_id = s.employee_id
   WHERE s.salary IS NULL
   ```
   — captures employees who are **missing salary info**.

2. The second part:
   ```sql
   SELECT s.employee_id
   FROM Salaries s
   LEFT JOIN Employees e ON s.employee_id = e.employee_id
   WHERE e.name IS NULL
   ```
   — captures employees who are **missing name info**.

3. `UNION` combines the two sets and automatically removes duplicates (if any), and `ORDER BY employee_id` sorts the result as required.

---
