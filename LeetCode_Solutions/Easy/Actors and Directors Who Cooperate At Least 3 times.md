### Actors and Directors Who Cooperate At Least 3 Times

**Table: ActorDirector**

```sql
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| actor_id    | int     |
| director_id | int     |
| timestamp   | int     |
+-------------+---------+
```

- `timestamp` is the **primary key** (unique for each row).
- Each row records a cooperation between an `actor_id` and a `director_id` at a certain time.

---

**Example Input:**

```sql
ActorDirector table:
+-------------+-------------+-------------+
| actor_id    | director_id | timestamp   |
+-------------+-------------+-------------+
| 1           | 1           | 0           |
| 1           | 1           | 1           |
| 1           | 1           | 2           |
| 1           | 2           | 3           |
| 1           | 2           | 4           |
| 2           | 1           | 5           |
| 2           | 1           | 6           |
+-------------+-------------+-------------+
```

---

**Goal:** Find all `(actor_id, director_id)` pairs who have cooperated **at least 3 times**.

```sql
SELECT 
  actor_id, 
  director_id 
FROM ActorDirector 
GROUP BY actor_id, director_id 
HAVING COUNT(*) >= 3;
```
