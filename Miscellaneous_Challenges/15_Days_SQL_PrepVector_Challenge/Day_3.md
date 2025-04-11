## Day 3: Single vs Repeat Job Posters
**Difficulty:** Medium

### Problem
Given a table of job postings, write a query to retrieve the number of users that have posted each job only once and the number of users that have posted at least one job multiple times.

---

### Given Data
```sql
CREATE TABLE job_postings (
    id INT PRIMARY KEY,
    user_id INT,
    job_id INT,
    posted_date DATETIME
);

INSERT INTO job_postings (id, user_id, job_id, posted_date) VALUES
    (1, 1, 101, '2024-01-01'),
    (2, 1, 102, '2024-01-02'),
    (3, 2, 201, '2024-01-01'),
    (4, 2, 201, '2024-01-15'),
    (5, 2, 202, '2024-01-03'),
    (6, 3, 301, '2024-01-01'),
    (7, 4, 401, '2024-01-01'),
    (8, 4, 401, '2024-01-15'),
    (9, 4, 402, '2024-01-02'),
    (10, 4, 402, '2024-01-16'),
    (11, 5, 501, '2024-01-05'),
    (12, 5, 502, '2024-01-10');
```

---

### My Solution
```sql
WITH grouped_postings AS (
  SELECT
    user_id,
    COUNT(DISTINCT job_id) AS num_jobs,
    COUNT(job_id) AS num_postings
  FROM job_postings
  GROUP BY user_id
)

SELECT
  COUNT(CASE WHEN num_postings = num_jobs THEN 1 END) AS single_post_users,
  COUNT(CASE WHEN num_postings != num_jobs THEN 1 END) AS multiple_post_users
FROM grouped_postings;
```

---
