# Day 1: Inactive Users Percentage
**Difficulty:** Easy

---

##  Problem Statement
You’re given two tables: `users` and `events`. The `events` table holds values of all of the user events in the `action` column (‘like’, ‘comment’, or ‘post’).

Write a query to get the **percentage of users that have never liked or commented**, rounded to two decimal places.

---

##  Schema & Sample Data
```sql
CREATE TABLE users (
  user_id INT PRIMARY KEY,
  name VARCHAR(50)
);

INSERT INTO users (user_id, name) VALUES
(1, 'John'),
(2, 'Jane'),
(3, 'Bob'),
(4, 'Alice'),
(5, 'Mike');

CREATE TABLE events (
  event_id INT PRIMARY KEY,
  user_id INT,
  action VARCHAR(10),
  created_at DATETIME,
  FOREIGN KEY (user_id) REFERENCES users(user_id)
);

INSERT INTO events (event_id, user_id, action, created_at) VALUES
(1, 1, 'post', '2024-01-01 10:00:00'),
(2, 1, 'post', '2024-01-01 14:00:00'),
(3, 1, 'post', '2024-01-02 09:00:00'),
(4, 2, 'like', '2024-01-01 10:05:00'),
(5, 2, 'comment', '2024-01-01 10:10:00'),
(6, 2, 'post', '2024-01-01 15:00:00'),
(7, 2, 'like', '2024-01-02 10:00:00'),
(8, 2, 'comment', '2024-01-02 10:30:00'),
(9, 3, 'post', '2024-01-01 11:00:00'),
(10, 3, 'post', '2024-01-02 13:00:00'),
(11, 3, 'post', '2024-01-03 09:00:00'),
(12, 4, 'post', '2024-01-02 09:00:00'),
(13, 4, 'post', '2024-01-02 16:00:00'),
(14, 4, 'post', '2024-01-03 11:00:00'),
(15, 5, 'like', '2024-01-02 14:00:00'),
(16, 5, 'post', '2024-01-03 10:00:00'),
(17, 5, 'like', '2024-01-03 15:00:00');
```

---

##  My Solution
```sql
WITH never AS ( 
    SELECT COUNT(*) AS never_liked_or_commented_users
    FROM users
    WHERE user_id NOT IN (
        SELECT DISTINCT user_id 
        FROM events 
        WHERE action IN ('like', 'comment')
    ) 
),
all_user AS (
    SELECT COUNT(DISTINCT user_id) AS all_count 
    FROM users  
)
SELECT 
    ROUND((n.never_liked_or_commented_users * 100.0) / a.all_count, 2) AS never_pct
FROM never n  
CROSS JOIN all_user a;
```

---

##  Solution from Others
```sql
WITH num_likes_and_comments AS(
  SELECT
     user_id,
     COUNT(CASE WHEN action = 'like' THEN 1 ELSE NULL END) AS number_likes,
     COUNT(CASE WHEN action = 'comment' THEN 1 ELSE NULL END) AS number_comments
  FROM events
  GROUP BY user_id
),
inactive AS (
  SELECT
    COUNT(user_id) AS user_id
    FROM num_likes_and_comments
    WHERE number_likes = 0 AND number_comments = 0
)
SELECT 
  user_id/5.0*100.00 AS percentage
FROM inactive;
```

---

## 💡 Key Learnings
- Use `NOT IN` or `LEFT JOIN` with `NULL` check to find non-participants.
- Cross join helps to avoid repeating subqueries when using aggregated values.
- Always round the final output as required.

---


