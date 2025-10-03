
---

# Day 13 of 15: The PrepVector SQL Challenge  
**Approach Used:** `CTE`, `CASE`, `NULLIF()`, `AGE BUCKETING`, `ORDER BY with Tie-Breaker`

---

## 📌 Problem Statement:

Given two tables, **search_events** and **users**, write a query to find the **three age groups** (bucketed by decade: 0-9, 10-19, 20-29, …, 80-89, 90-99, with the end point included) with the **highest clickthrough rate in 2021**.  

If two or more groups have the same clickthrough rate, the **older group should have priority**.

---

## 🧠 Thought Process:

When I saw the problem, my first question was:  

**How do I track clickthrough rate by age group when age isn’t directly available?**

---

### ➟ **Step 1: Calculating Age at the Time of Event**  
I realized we had `birthdate` and `search_time`, so I just subtracted the year of birth from the year of search to get the user's age at the time of the event.  
Simple logic, but very effective.

---

### ➟ **Step 2: Grouping into Age Buckets**  
The question asked for buckets like 0–9, 10–19, and so on.  
So I divided age by 10 and floored it using `CAST(age / 10 AS INTEGER)`, now each user had a clean age group.

---

### ➟ **Step 3: Calculating CTR (Clickthrough Rate)**  
Now came the main metric.  
I used `CASE WHEN has_clicked THEN 1 ELSE 0` to count clicks and divided it by the total number of events per group.

---

### ➟ **Final Step: Sorting with a Twist**  
Here’s where the twist came in → If CTR was tied between groups, the older group should win.  
So I added `ORDER BY ctr DESC, age_groups DESC`, giving priority to older users in case of a tie.  

And finally, just picked the **Top 3**.

---

## ✅ SQL Query Solution:
```sql
WITH events_2021 AS (
    SELECT
        se.user_id,
        se.has_clicked,
        se.search_time,
        u.birthdate,
        CAST(strftime('%Y', se.search_time) AS INTEGER) - CAST(strftime('%Y', u.birthdate) AS INTEGER) AS age_at_search
    FROM search_events se
    JOIN users u ON se.user_id = u.id
    WHERE strftime('%Y', se.search_time) = '2021'
),
age_bucketed AS (
    SELECT
        user_id,
        has_clicked,
        search_time,
        age_at_search,
        CAST(age_at_search / 10 AS INTEGER) AS age_group
    FROM events_2021
),
ctr_by_age_group AS (
    SELECT
        age_group,
        ROUND(100.0 * SUM(CASE WHEN has_clicked THEN 1 ELSE 0 END) / NULLIF(COUNT(*), 0), 2) AS ctr
    FROM age_bucketed
    GROUP BY age_group
)
SELECT
    CAST(age_group * 10 AS TEXT) || '-' || CAST((age_group * 10 + 9) AS TEXT) AS age_groups,
    ctr
FROM ctr_by_age_group
ORDER BY ctr DESC, age_groups DESC
LIMIT 3;
```

---

## 🔑 Key Takeaway:

• Whenever you're asked to analyze metrics across age groups,  
Use simple math:  
**Year of Event - Year of Birth = Age at Event**  

• For CTR (or any ratio), always protect against division by zero using `NULLIF()`.

---

