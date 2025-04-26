### Number of Unique Subjects Taught by Each Teacher

**Write a solution to calculate the number of unique subjects each teacher teaches in the university.**

Return the result table in **any order**.

---

**Input:**

```sql
Teacher table:
+------------+------------+---------+
| teacher_id | subject_id | dept_id |
+------------+------------+---------+
| 1          | 2          | 3       |
| 1          | 2          | 4       |
| 1          | 3          | 3       |
| 2          | 1          | 1       |
| 2          | 2          | 1       |
| 2          | 3          | 1       |
| 2          | 4          | 1       |
+------------+------------+---------+
```

---

**Query:**

```sql
SELECT 
  teacher_id, 
  COUNT(DISTINCT subject_id) AS cnt
FROM Teacher 
GROUP BY teacher_id;
```

--- 

