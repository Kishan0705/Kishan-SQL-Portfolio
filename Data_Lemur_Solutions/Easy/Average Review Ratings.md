## Problem Statement :

### Average Review Ratings  
**Amazon SQL Interview Question**

Given the `reviews` table, write a query to retrieve the average star rating for each product, grouped by month. The output should display:
- The month as a numerical value
- Product ID
- Average star rating rounded to two decimal places  

Sort the output first by month and then by product ID.

---

### SQL Query:

```sql
SELECT
    EXTRACT(MONTH FROM submit_date) AS mth,
    product_id,
    ROUND(AVG(stars), 2) AS avg_stars
FROM reviews
GROUP BY mth, product_id
ORDER BY mth, product_id;
```

---

