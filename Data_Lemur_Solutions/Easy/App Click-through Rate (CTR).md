## Problem Statement:  

### **App Click-through Rate (CTR)**  
**Facebook SQL Interview Question**  

Assume you have an events table on Facebook app analytics. Write a query to calculate the click-through rate (CTR) for the app in 2022 and round the results to 2 decimal places.

### **Definition and Note:**
- **Percentage of click-through rate (CTR)** = `(100.0 * Number of clicks) / Number of impressions`
- To avoid integer division, multiply the CTR by `100.0`, not `100`.

---

### **SQL Query:**
#### **FILTER Approach (PostgreSQL Specific)**  
```sql
SELECT 
    app_id,
    ROUND(
        (100.0 * COUNT(*) FILTER (WHERE event_type = 'click')) / 
        NULLIF(COUNT(*) FILTER (WHERE event_type = 'impression'), 0), 
        2
    ) AS ctr
FROM events 
WHERE EXTRACT(YEAR FROM timestamp) = 2022
GROUP BY app_id;
```

#### **CASE WHEN Approach (General SQL)**  
```sql
SELECT 
    app_id,
    ROUND(
        (100.0 * SUM(CASE WHEN event_type = 'click' THEN 1 END)) / 
        SUM(CASE WHEN event_type = 'impression' THEN 1 END), 
        2
    ) AS ctr
FROM events 
WHERE EXTRACT(YEAR FROM timestamp) = 2022
GROUP BY app_id;
