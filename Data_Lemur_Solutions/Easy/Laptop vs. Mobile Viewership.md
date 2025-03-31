## Problem Statement  

### **Laptop vs. Mobile Viewership**  
**NY Times SQL Interview Question**  

Assume you're given a table on user viewership categorized by device type where the three types are **laptop, tablet, and phone**.  

Write a query that calculates the total viewership for **laptops** and **mobile devices**, where mobile is defined as the sum of **tablet and phone viewership**. Output the total viewership for laptops as `laptop_views` and the total viewership for mobile devices as `mobile_views`.  

---

## **SQL Query**  

### **Standard Approach (Using SUM & CASE)**  
```sql
SELECT
    SUM(CASE WHEN device_type = 'laptop' THEN 1 END) AS laptop_views,
    SUM(CASE WHEN device_type IN ('tablet', 'phone') THEN 1 END) AS mobile_views
FROM viewership;
```

---

### **Alternative Approach (PostgreSQL-Specific Using FILTER)**  
```sql
SELECT
    COUNT(*) FILTER (WHERE device_type = 'laptop') AS laptop_views,
    COUNT(*) FILTER (WHERE device_type IN ('tablet', 'phone')) AS mobile_views
FROM viewership;
```

---

