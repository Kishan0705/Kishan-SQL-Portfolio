# Histogram of Tweets

## Twitter SQL Interview Question

### Problem Statement

You are given a table containing Twitter tweet data. Write a SQL query to obtain a **histogram** of tweets posted per user in **2022**.

**Objective:**

- Output the **tweet count per user** as the bucket.
- Count the **number of Twitter users** who fall into each bucket.
- In other words, group users by the number of tweets they posted in 2022 and count the number of users in each group.

---

## CTE Solution

```sql
WITH tweet_counts AS (
    SELECT 
        user_id,
        COUNT(tweet_id) AS no_of_tweets
    FROM tweets 
    WHERE EXTRACT(YEAR FROM tweet_date) = 2022
    GROUP BY user_id
) 
SELECT 
    no_of_tweets AS tweet_bucket,
    COUNT(user_id) AS users_num
FROM tweet_counts 
GROUP BY tweet_bucket;
```

---

## Subquery Solution

```sql
SELECT 
    no_of_tweets AS tweet_bucket, 
    COUNT(user_id) AS users_num
FROM (
    SELECT 
        user_id, 
        COUNT(tweet_id) AS no_of_tweets
    FROM tweets 
    WHERE EXTRACT(YEAR FROM tweet_date) = 2022
    GROUP BY user_id
) AS user_tweet_counts
GROUP BY tweet_bucket;
```

---


