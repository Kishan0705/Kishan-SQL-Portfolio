## Day 6: Above Average Product Prices
**Difficulty:** Medium

### Problem
Given a table of transactions and products, write a query to return the product ID, product price, and average transaction price of all products with a price greater than the average transaction price.

---

### Given Data

```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    price DECIMAL(10,2)
);

INSERT INTO products (product_id, price) VALUES
(1, 100.00),
(2, 150.00),
(3, 75.00),
(4, 200.00),
(5, 120.00);

CREATE TABLE transactions (
    transaction_id INT PRIMARY KEY,
    product_id INT,
    amount DECIMAL(10,2),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);

INSERT INTO transactions (transaction_id, product_id, amount) VALUES
(1, 1, 95.00),
(2, 1, 98.00),
(3, 2, 145.00),
(4, 2, 150.00),
(5, 3, 70.00),
(6, 4, 190.00),
(7, 4, 195.00),
(8, 5, 115.00);
```

---

### Solution

```sql
WITH avg_transaction_price AS (
    SELECT
        product_id,
        ROUND(AVG(amount),2) AS avg_price
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
