## Problem Statement

### Page With No Likes  
**Facebook SQL Interview Question**  

Assume you're given two tables containing data about Facebook Pages and their respective likes (as in "Like a Facebook Page").

Write a query to return the IDs of the Facebook pages that have zero likes. The output should be sorted in ascending order based on the page IDs.

---

## SQL Solutions

### **NOT IN Approach**  
```sql
SELECT 
    page_id 
FROM pages 
WHERE page_id NOT IN ( 
    SELECT 
        page_id
    FROM page_likes)
ORDER BY page_id;
```

### **LEFT JOIN and IS NULL Approach**  
```sql
SELECT p.page_id 
FROM pages p
LEFT JOIN page_likes pl ON p.page_id = pl.page_id
WHERE pl.page_id IS NULL
ORDER BY p.page_id;
```

### **NOT EXISTS Approach**  
```sql
SELECT page_id 
FROM pages p
WHERE NOT EXISTS (
    SELECT 1 
    FROM page_likes pl 
    WHERE p.page_id = pl.page_id
)
ORDER BY page_id;
```

### **EXCEPT Approach**  
```sql
SELECT page_id 
FROM pages
EXCEPT
SELECT page_id 
FROM page_likes
ORDER BY page_id;
