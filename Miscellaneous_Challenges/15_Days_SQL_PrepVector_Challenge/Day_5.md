## Day 5: Post Completion Rate
**Difficulty:** Medium

### Problem
Consider the `events` table, which contains information about the phases of writing a new social media post.

The `action` column can have values:
- `post_enter`: when a user starts to write a post
- `post_submit`: when a user submits a post
- `post_canceled`: when a user cancels a post

Write a query to get the **post-success rate** for each day in the month of **January 2020**.

> **Note:** Post Success Rate = (number of `post_submit`) / (number of `post_enter`) per day.

---

### Given Data
```sql
CREATE TABLE events (
  user_id INT,
  created_at DATETIME,
  action VARCHAR(20)
);

INSERT INTO events VALUES
  (1, '2020-01-01 10:00:00', 'post_enter'),
  (1, '2020-01-01 10:05:00', 'post_submit'),
  (2, '2020-01-01 11:00:00', 'post_enter'),
  (2, '2020-01-01 11:10:00', 'post_canceled'),
  (3, '2020-01-01 15:00:00', 'post_enter'),
  (3, '2020-01-01 15:30:00', 'post_submit'),
  (4, '2020-01-02 09:00:00', 'post_enter'),
  (4, '2020-01-02 09:15:00', 'post_canceled'),
  (5, '2020-01-02 10:00:00', 'post_enter'),
  (5, '2020-01-02 10:10:00', 'post_canceled'),
  (10, '2020-01-15 14:00:00', 'post_enter'),
  (10, '2020-01-15 14:30:00', 'post_submit'),
  (6, '2019-12-31 23:55:00', 'post_enter'),
  (6, '2020-01-01 00:05:00', 'post_submit'),
  (7, '2020-02-01 00:00:00', 'post_enter'),
  (7, '2020-02-01 00:10:00', 'post_submit'),
  (8, '2019-01-15 10:00:00', 'post_enter'),
  (8, '2019-01-15 10:30:00', 'post_submit'),
  (9, '2021-01-01 09:00:00', 'post_enter'),
  (9, '2021-01-01 09:10:00', 'post_canceled');
```

---

### SQL Query
```sql
WITH daily_counts AS (
  SELECT
    DATE(created_at) AS Dates,
    SUM(CASE WHEN action = 'post_enter' THEN 1 ELSE 0 END) AS total_post_enter,
    SUM(CASE WHEN action = 'post_submit' THEN 1 ELSE 0 END) AS total_post_submit
  FROM events
  WHERE strftime('%Y-%m', created_at) = '2020-01'
  GROUP BY Dates
)
SELECT
  Dates,
  total_post_enter,
  total_post_submit,
  ROUND((total_post_submit * 100.0) / NULLIF(total_post_enter, 0), 2) AS success_rate
FROM daily_counts;
```

---

### Time Intelligence Functions by RDBMS
**SQLite**
```sql
strftime('%Y-%m', created_at) = '2020-01'
```

**MySQL**
```sql
DATE_FORMAT(created_at, '%Y-%m') = '2020-01'
-- OR --
WHERE EXTRACT(YEAR FROM created_at) = 2020
  AND EXTRACT(MONTH FROM created_at) = 1
```

**PostgreSQL**
```sql
TO_CHAR(created_at, 'YYYY-MM') = '2020-01'
-- OR --
WHERE EXTRACT(YEAR FROM created_at) = 2020
  AND EXTRACT(MONTH FROM created_at) = 1
```

