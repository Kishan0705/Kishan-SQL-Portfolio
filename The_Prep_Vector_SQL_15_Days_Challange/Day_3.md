
---

# Day 3 of 15: The PrepVector SQL Challenge  
**Approach Used:** `CTE`, `CASE`, `COUNT`, `GROUP BY`

---

## 📌 Problem Statement  
Need to find:  
- **Number of users who have posted each job only once**  
- **Number of users that have posted at least one job multiple times**  

---

## 🧠 Thought Process  
- Initially, I counted jobs posted only once and multiple times, but this didn’t ensure we were counting **unique users** who met these conditions.  
- The question asks for **number of users**, not just job counts.  
- So, I:  
  1. Counted job postings per user for each job.  
  2. Grouped users based on whether they had repeated any job.  
  3. Ensured we count **users correctly** instead of just job occurrences.  

---

## ✅ SQL Query Solution  
```sql
WITH user_job_counts AS (
    SELECT
        user_id,
        job_id,
        COUNT(*) AS post_count
    FROM job_postings
    GROUP BY user_id, job_id
),
users_grouped AS (
    SELECT
        user_id,
        COUNT(CASE WHEN post_count = 1 THEN 1 END) AS single_post_jobs,
        COUNT(CASE WHEN post_count > 1 THEN 1 END) AS multiple_post_jobs
    FROM user_job_counts
    GROUP BY user_id
)
SELECT
    COUNT(CASE WHEN multiple_post_jobs = 0 THEN 1 END) AS users_with_only_unique_jobs,
    COUNT(CASE WHEN multiple_post_jobs > 0 THEN 1 END) AS users_with_repeated_jobs
FROM users_grouped;
```

---

### 🔑 Key Takeaways  
✔️ Used **COUNT() with CASE** to categorize users based on job posting frequency.  
✔️ Ensured we count **unique users** rather than just job occurrences.  
✔️ Applied **CTEs** to break down the problem into logical steps for better clarity.  
✔️ Identified users who have only **unique job posts** vs. those with **repeated job postings**.  

---

