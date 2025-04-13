
### **Day 14: Monthly Revenue Growth**  
**Difficulty**: Hard

---

### **Problem Statement**

You are given two tables:

- `transactions(id, product_id, quantity, created_at)`
- `products(id, price)`

Your task is to calculate the **Month-over-Month (MoM) revenue growth** for the year **2019**.



The result should be **rounded to two decimal places**. The output should include each month and its corresponding revenue growth.

---


### **SQL Query**

```sql
WITH cte AS (
  SELECT 
    CAST(strftime('%m', t.created_at) AS INTEGER) AS months,
    SUM(p.price * t.quantity) AS total_sales
  FROM products p 
  JOIN transactions t ON p.id = t.product_id
  WHERE strftime('%Y', t.created_at) = '2019'
  GROUP BY months
)
SELECT 
  months,
  ROUND(
    (total_sales - LAG(total_sales) OVER (ORDER BY months)) * 100.0 / 
    LAG(total_sales) OVER (ORDER BY months), 
    2
  ) AS month_over_month
FROM cte;
```

---


