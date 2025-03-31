## Problem Statement  

### **Average Post Hiatus (Part 1)**  
**Facebook SQL Interview Question**  

Given a table of Facebook posts, for each user who posted at least twice in 2021, write a query to find the number of days between each user’s first post of the year and last post of the year in 2021. Output the user and the number of days between each user's first and last post.

---

### **SQL Query**  
```sql
SELECT 
    user_id,
    CAST(MAX(post_date) AS DATE) - 
    CAST(MIN(post_date) AS DATE) AS Date_Diff 
FROM posts 
WHERE EXTRACT(YEAR FROM post_date) = 2021
GROUP BY user_id
HAVING COUNT(post_id) > 1;
