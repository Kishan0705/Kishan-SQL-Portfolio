
---

# Day 11 of 15: The PrepVector SQL Challenge  
**Approach Used:** `ROW_NUMBER()`, `MIN()`, `GROUP BY`, `CTE`, `Window Functions`

---

## 📌 Problem Statement:

Write a query to extract the earliest date each user played their **third unique song** and order by date played.

---

## 🧠 Thought Process:

Initially, I was getting duplicate song IDs in my output, and that really confused me. I just wanted the unique ones each user played.  

Then I realized the trick: use **MIN()** on the `played_at` column and **GROUP BY** both `user_id` and `song_id`.  

That gave me the first time each song was played by each user.  

After that, it was pretty simple:  

I just applied **ROW_NUMBER()** to rank the plays and filtered out the 3rd song for each user.

---

## ✅ SQL Query Solution:
```sql
WITH song_ranking AS (
    SELECT
        user_id,
        song_id,
        first_played,
        ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY first_played) AS rnk
    FROM (
        SELECT
            user_id,
            song_id,
            MIN(played_at) AS first_played
        FROM song_plays
        GROUP BY user_id, song_id
    ) AS user_first_played_song
)
SELECT
    u.username,
    s.song_id,
    s.first_played
FROM song_ranking s
JOIN users u ON s.user_id = u.id
WHERE rnk = 3
ORDER BY first_played;
```

---

## 🔑 Key Takeaway:

• Whenever you get a problem statement where you’re asked to get the **Nth something**, the first idea that should hit your mind is: **WINDOW FUNCTION** 💡  

• In this query specifically:  
Using **ROW_NUMBER()** after identifying each user’s first play per song helped me pull out exactly the 3rd song with ease.  

• Also, always try to break the problem into layers.  
Here, figuring out the first time each song was played before applying the rank made everything way cleaner and easier to handle.

---

