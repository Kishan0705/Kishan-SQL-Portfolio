### Product Sales Analysis I

**Table: Sales**

```sql
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+
```

- `(sale_id, year)` is the **primary key**.
- `product_id` is a **foreign key** referring to the `Product` table.
- Each row represents the sale of a product in a specific year.
- `price` is per unit.

---

**Table: Product**

```sql
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+
```

- `product_id` is the **primary key**.
- Contains the name of each product.

---

**Goal:** Report the `product_name`, `year`, and `price` for each sale in the `Sales` table.

```sql
SELECT 
  p.product_name, 
  s.year, 
  s.price 
FROM Sales s 
LEFT JOIN Product p 
  ON s.product_id = p.product_id;
```
