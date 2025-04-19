### Project Employees I

**Table: Project**

```sql
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| project_id  | int     |
| employee_id | int     |
+-------------+---------+
```

- `(project_id, employee_id)` is the **primary key**.
- `employee_id` is a **foreign key** referring to the `Employee` table.
- Each row shows which employee is working on which project.

---

**Table: Employee**

```sql
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| employee_id      | int     |
| name             | varchar |
| experience_years | int     |
+------------------+---------+
```

- `employee_id` is the **primary key**.
- Contains information about each employee.
- `experience_years` is guaranteed to be **NOT NULL**.

---

**Goal:** Report the average experience (rounded to 2 decimal places) of all employees per project.

```sql
SELECT 
  p.project_id,
  ROUND(AVG(e.experience_years), 2) AS average_years 
FROM Project p 
JOIN Employee e 
  ON p.employee_id = e.employee_id
GROUP BY p.project_id;
```
