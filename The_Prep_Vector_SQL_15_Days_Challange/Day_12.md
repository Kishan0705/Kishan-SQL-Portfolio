
---

# Day 12 of 15: The PrepVector SQL Challenge  
**Approach Used:** `ROW_NUMBER()`, `DATE Trick`, `CTE`, `GROUP BY`, `MAX()`

---

## 📌 Problem Statement:

Given a table with event logs, find the **top five users with the longest continuous streak of visiting the platform in 2020**.  

**Note:** A continuous streak counts if the user visits the platform at least once per day on consecutive days.

---

## 🧠 Thought Process:

First question came into my mind when I started solving this query was:  

**How can I find consecutive days?** Because the same user can have multiple separate streaks! ❓  

Then I followed a **Step-by-Step approach** to tackle this question:  

---

### ➟ **Step 1: Filtered the Data**  
I first extracted the events that occurred in 2020 and made sure to get **unique dates per user**. This helped remove multiple events on the same day.

---

### ➟ **Step 2: Apply Row Number**  
This was key! I used `ROW_NUMBER()` to assign a rank to each user's activity, ordered by the event date. Now I had a clean way to measure how many days in a row they were active.

---

### ➟ 💥 **Step 3: The MAGIC Formula**  
```
DATE(event_date, '-' || rnk || ' days')
```
This formula normalizes each streak to a common **base date**. Basically, if users were active 3 days in a row, subtracting the row number aligns all those dates to the same base.  

So now, consecutive dates share the same "base", which makes it easy to **GROUP and COUNT them = STREAK FOUND 😎**

---

### ➟ ✅ **Final Step:**  
I grouped the streaks and found the maximum for each user. Then simply picked the **Top 5 users with the longest streaks**.

---

## ✅ SQL Query Solution:
```sql
WITH filtered_events AS (
    SELECT
        user_id,
        DATE(created_at) AS event_date
    FROM events
    WHERE strftime('%Y', created_at) = '2020'
    GROUP BY user_id, DATE(created_at)
),
event_ranks AS (
    SELECT
        user_id,
        event_date,
        ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY event_date) AS rnk
    FROM filtered_events
),
grouped_streaks AS (
    SELECT
        user_id,
        event_date,
        rnk,
        DATE(event_date, '-' || rnk || ' days') AS base
    FROM event_ranks
),
streak_lengths AS (
    SELECT
        user_id,
        base,
        COUNT(*) AS streak_length
    FROM grouped_streaks
    GROUP BY user_id, base
),
max_streaklength_per_user AS (
    SELECT
        user_id,
        MAX(streak_length) AS max_streak
    FROM streak_lengths
    GROUP BY user_id
)
SELECT
    user_id,
    max_streak
FROM max_streaklength_per_user
ORDER BY max_streak DESC, user_id
LIMIT 5;
```

---

## 🔑 Key Takeaway:

• When the problem asks for **"longest streaks" or "consecutive days"**, your first instinct should be:  
👉 **Row Number + Date Trick is the combo you need!**  

• This little hack → `event_date - row_number` creates a magic **base date** that helps group consecutive days together seamlessly.  

• Always think in layers:  
First, **clean your data → then rank it → then transform it → and only then go for aggregation**.

---

