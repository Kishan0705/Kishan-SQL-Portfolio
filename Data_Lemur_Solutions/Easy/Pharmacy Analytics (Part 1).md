## Problem Statement:

### Pharmacy Analytics (Part 1)  
**CVS Health SQL Interview Question**

CVS Health is trying to better understand its pharmacy sales and how well different products are selling. Each drug can only be produced by one manufacturer.

Write a query to find the **top 3 most profitable drugs** sold and how much profit they made. Assume that there are no ties in the profits. Display the result from the highest to the lowest total profit.

### Definition:
- **COGS (Cost of Goods Sold)**: The direct cost associated with producing the drug.
- **Total Profit** = Total Sales - Cost of Goods Sold

### SQL Query:
```sql
SELECT 
    drug, 
    (SUM(total_sales) - SUM(cogs)) AS profit 
FROM pharmacy_sales 
GROUP BY drug 
ORDER BY profit DESC 
LIMIT 3;
