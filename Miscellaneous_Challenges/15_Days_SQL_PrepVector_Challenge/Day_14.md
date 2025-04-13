
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
```sql

Given Code

CREATE TABLE transactions (
id INT PRIMARY KEY,
product_id INT,
quantity INT,
created_at TIMESTAMP,
FOREIGN KEY (product_id) REFERENCES products(id)
);

INSERT INTO transactions (id, product_id, quantity, created_at) VALUES
(1, 101, 2, '2019-01-15 10:00:00'),
(2, 102, 1, '2019-01-20 12:30:00'),
(3, 101, 3, '2019-02-10 14:00:00'),
(4, 103, 1, '2019-02-25 16:15:00'),
(5, 102, 4, '2019-03-05 09:30:00'),
(6, 101, 1, '2019-03-18 13:45:00');

CREATE TABLE products (
id INT PRIMARY KEY,
price DECIMAL(10, 2)
);

INSERT INTO products (id, price) VALUES
(101, 20.00),
(102, 15.00),
(103, 30.00);


```



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


