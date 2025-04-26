### Calculate Special Bonus

Write a solution to **calculate the bonus** of each employee.

- Bonus = **100% of salary** if:
  - The employee's **ID is odd**
  - The employee's **name does not start with 'M'**
- Otherwise, bonus = **0**

---

**Input:**

```sql
Employees table:
+-------------+---------+--------+
| employee_id | name    | salary |
+-------------+---------+--------+
| 2           | Meir    | 3000   |
| 3           | Michael | 3800   |
| 7           | Addilyn | 7400   |
| 8           | Juan    | 6100   |
| 9           | Kannon  | 7700   |
+-------------+---------+--------+
```

---

**Query:**

```sql
SELECT 
  employee_id, 
  CASE 
    WHEN employee_id % 2 = 1 AND name NOT LIKE 'M%' 
    THEN salary 
    ELSE 0 
  END AS bonus
FROM Employees
ORDER BY employee_id;
```

---

**✅ Output:**

```sql
+-------------+-------+
| employee_id | bonus |
+-------------+-------+
| 2           | 0     |
| 3           | 0     |
| 7           | 7400  |
| 8           | 0     |
| 9           | 7700  |
+-------------+-------+
```

---
