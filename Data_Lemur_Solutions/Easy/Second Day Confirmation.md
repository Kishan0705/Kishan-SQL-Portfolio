## Problem Statement :

### Second Day Confirmation  
**TikTok SQL Interview Question**  

Assume you're given tables with information about TikTok user sign-ups and confirmations through email and text. New users on TikTok sign up using their email addresses, and upon sign-up, each user receives a text message confirmation to activate their account.

Write a query to display the user IDs of those who did not confirm their sign-up on the first day but confirmed on the second day.

#### Definition:
- `action_date` refers to the date when users activated their accounts and confirmed their sign-up through text messages.

### SQL Queries:

#### **Using CTE & ROW_NUMBER**
```sql
WITH cte AS (
    SELECT 
        e.user_id,
        e.email_id,
        t.signup_action,
        t.action_date,
        ROW_NUMBER() OVER (PARTITION BY t.email_id ORDER BY action_date) AS rnk 
    FROM emails e 
    JOIN texts t ON e.email_id = t.email_id
)
SELECT user_id
FROM cte 
WHERE rnk = 2 AND signup_action = 'Confirmed';
```

#### **Using INTERVAL (Short & Efficient Approach)**
```sql
SELECT DISTINCT user_id
FROM emails 
JOIN texts ON emails.email_id = texts.email_id
WHERE texts.action_date = emails.signup_date + INTERVAL '1 day'
  AND texts.signup_action = 'Confirmed';
