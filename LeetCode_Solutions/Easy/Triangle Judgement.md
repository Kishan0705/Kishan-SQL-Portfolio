### Triangle Judgement

**Table: Triangle**


| Column Name | Type |
|-------------|------|
| x           | int  |
| y           | int  |
| z           | int  |


In SQL, (x, y, z) is the primary key column for this table.  
Each row of this table contains the lengths of three line segments.

---

**Write a solution to report for every three line segments whether they can form a triangle.**

```sql
SELECT 
  x, y, z,
  CASE 
    WHEN x + y > z AND x + z > y AND y + z > x THEN 'Yes'
    ELSE 'No'
  END AS triangle
FROM Triangle;
