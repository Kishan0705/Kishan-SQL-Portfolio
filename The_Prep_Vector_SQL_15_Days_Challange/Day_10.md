
---

# Day 10 of 15: The PrepVector SQL Challenge  
**Approach Used:** `SELF JOIN`, `COUNT(DISTINCT)`, `GROUP BY`

---

## 📌 Problem Statement:

A dating website’s schema is represented by a table of people that like other people. The table has three columns:  
- `user_id`  
- `liker_id` (the user id of the user doing the liking)  
- `created_at` (the date time that the like occurred)  

Write a query to count the number of liker’s likers (the users that like the likers) if the liker has one.

---

## 🧠 Thought Process:

❌ **Confusion Phase** ❌  

First I got confused at *"how can I find liker's liker?"* ❓ - on top of that *"if the liker has one"* 🤔  

This confusion happened because I **MISUNDERSTOOD** the actual meaning of column `liker_id`.  

I initially thought `liker_id` was a like id (felt pretty dumb realizing it later 😅).  

Because of these misunderstandings, I tried to find users' like count.  

✅ **Finally UNDERSTOOD the requirement** ✅  

When I carefully went through the problem statement, I realized:  

- `liker_id` is also a kind of `user_id`.  
- We need to find which user has liked the liker’s account.  

So actually, we need to find reversely — how many users have liked the likers.  

That’s when I confirmed — a **SELF JOIN** was a must here.  

Eventually, I self-joined the table where the `liker_id` matched a `user_id`, and then counted the distinct liker ids.

---

## ✅ SQL Query Solution:
```sql
SELECT
    l.liker_id,
    COUNT(DISTINCT l2.liker_id) AS cnt
FROM likes l
JOIN likes l2
    ON l.liker_id = l2.user_id
GROUP BY l.liker_id;
```

---

## ❌ Incorrect Attempts:
```sql
-- Attempt 1 (Misunderstood requirement)
SELECT
    user_id,
    COUNT(liker_id) AS cnt
FROM likes
GROUP BY user_id
HAVING COUNT(liker_id) > 0;

-- Attempt 2 (Still wrong interpretation)
SELECT
    liker_id,
    COUNT(DISTINCT user_id) AS cnt
FROM likes
GROUP BY liker_id
HAVING COUNT(DISTINCT user_id) > 0;
```

---

## 🔑 Key Takeaway:

• Knowing **HOW TO self join** the table based on actual requirement can indeed make things easier.  
• When a question has **twisted wordings** — DO NOT make your own assumptions, **READ the question properly** in order to solve it efficiently.

---

