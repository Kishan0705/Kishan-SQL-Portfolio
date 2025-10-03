
---

# Day 6 of 15: The PrepVector SQL Challenge  
**Approach Used:** `CTE`, `AVG()`, `ROUND()`, `JOIN`, `GROUP BY`

---

## 📌 Problem Statement:

Write a query to return the **product ID**, **product price**, and **average transaction price** of all products with a price greater than the average transaction price.

---

## 🧠 Thought Process:

• First, I calculated the average transaction price for each product using `AVG(amount)`, rounding it to two decimal places.  

• Then, I used a **CTE** to keep the query structured and easy to read.  

• Finally, I joined the CTE with the products table and filtered for products where the listed price is higher than the average transaction price.

---

## ✅ SQL Query Solution:
```sql
WITH avg_transaction_price AS (
    SELECT
        product_id,
        ROUND(AVG(amount), 2) AS avg_price
    FROM transactions
    GROUP BY product_id
)
SELECT
    p.product_id,
    p.price AS product_price,
    atp.avg_price AS average_transaction_price
FROM products p
JOIN avg_transaction_price atp
    ON p.product_id = atp.product_id
WHERE p.price > atp.avg_price;
```

---

## 🔑 Key Takeaway:

• This query helps identify products that are priced higher than their average transaction price, which can be useful for pricing analysis, detecting overpriced products, or understanding customer purchasing behavior.

---

