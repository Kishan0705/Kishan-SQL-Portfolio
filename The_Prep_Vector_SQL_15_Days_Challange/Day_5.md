

---

# Day 5 of 15: The PrepVector SQL Challenge  
**Approach Used:** `CTE`, `CASE`, `GROUP BY`, `NULLIF`, `ROUND`

---

## 📌 Problem Statement:

▸Given the events table, which logs user actions during the process of creating a social media post, determine the daily post-success rate for January 2020.  
▸The action column records whether a user started writing, submitted the post, or canceled it.  
▸Calculate the success rate as the percentage of submitted posts out of the total posts started for each day.

---

## 🧠 Thought Process:

▸I first identified the key metrics, counting post enter and post submit for each day.  
▸Then, I filtered the data for January 2020 using `strftime('%Y-%m', created_at) = '2020-01'`.  
▸Next, I grouped by date to sum up the total posts started and submitted.  
▸To calculate the success rate, I divided total post submit by total post enter, handling division by zero with `NULLIF`.  
▸Finally, using a CTE, I kept the query structured and readable, making the final selection easier.

---

## ✅ SQL Query Solution:
```sql
WITH daily_counts AS (
    SELECT
        DATE(created_at) AS day,
        COUNT(CASE WHEN action = 'post_enter' THEN 1 END) AS started_posts,
        COUNT(CASE WHEN action = 'post_submit' THEN 1 END) AS submitted_posts
    FROM events
    WHERE strftime('%Y-%m', created_at) = '2020-01'
    GROUP BY DATE(created_at)
)
SELECT
    day,
    ROUND((submitted_posts * 100.0) / NULLIF(started_posts, 0), 2) AS success_rate
FROM daily_counts
ORDER BY day;
```

---

## 🔑 Key Takeaway:

✔️ Using `NULLIF` helped avoid division errors, making the query more reliable and accurate for tracking user activity.

---


