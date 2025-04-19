### Swap Salary

**Table: Salary**

```sql
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| id          | int      |
| name        | varchar  |
| sex         | ENUM     |
| salary      | int      |
+-------------+----------+
```

- `id` is the primary key (column with unique values) for this table.  
- The `sex` column is an ENUM with values `'m'` and `'f'`.  
- The table contains information about an employee.

Write a solution to **swap all `'f'` and `'m'` values** with a **single UPDATE statement**, and **no intermediate temporary tables** or SELECT statements.

```sql
UPDATE Salary
SET sex = CASE 
            WHEN sex = 'm' THEN 'f'
            WHEN sex = 'f' THEN 'm'
          END;
```
