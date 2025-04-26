###  Replace Employee ID with Unique Identifier

**Write a solution to report the distance traveled by each user.**

Return the result table ordered by `travelled_distance` in descending order.  
If two or more users traveled the same distance, order them by their `name` in ascending order.

---

**Input:**

```sql
Users table:
+------+-----------+
| id   | name      |
+------+-----------+
| 1    | Alice     |
| 2    | Bob       |
| 3    | Alex      |
| 4    | Donald    |
| 7    | Lee       |
| 13   | Jonathan  |
| 19   | Elvis     |
+------+-----------+

Rides table:
+------+----------+----------+
| id   | user_id  | distance |
+------+----------+----------+
| 1    | 1        | 120      |
| 2    | 2        | 317      |
| 3    | 3        | 222      |
| 4    | 7        | 100      |
| 5    | 13       | 312      |
| 6    | 19       | 50       |
| 7    | 7        | 120      |
| 8    | 19       | 400      |
| 9    | 7        | 230      |
+------+----------+----------+
```

---

**Query:**

```sql
SELECT 
  u.name, 
  COALESCE(SUM(r.distance), 0) AS travelled_distance 
FROM Users u 
LEFT JOIN Rides r 
  ON u.id = r.user_id 
GROUP BY u.id
ORDER BY travelled_distance DESC, u.name;
```
