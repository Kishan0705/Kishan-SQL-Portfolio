# SQL Problem: Sales Person

## Problem Statement

You are given the following tables: `SalesPerson`, `Company`, and `Orders`.

### Table: SalesPerson

| Column Name       | Type    |
|-------------------|---------|
| sales_id          | int     |
| name              | varchar |
| salary            | int     |
| commission_rate   | int     |
| hire_date         | date    |

`sales_id` is the primary key.

---

### Table: Company

| Column Name | Type    |
|-------------|---------|
| com_id      | int     |
| name        | varchar |
| city        | varchar |

`com_id` is the primary key.

---

### Table: Orders

| Column Name | Type    |
|-------------|---------|
| order_id    | int     |
| order_date  | date    |
| com_id      | int     |
| sales_id    | int     |
| amount      | int     |

`order_id` is the primary key.

## Objective

Write a solution to find the names of all the salespersons who did **not** have any orders related to the company with the name **"RED"**.

## SQL Query

```sql
SELECT 
  name 
FROM SalesPerson 
WHERE sales_id NOT IN (
  SELECT o.sales_id 
  FROM Orders o 
  JOIN Company c 
    ON o.com_id = c.com_id 
  WHERE c.name = 'RED'
);
