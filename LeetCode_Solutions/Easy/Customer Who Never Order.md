# SQL Problem: Customer Who Never Order

## Problem Statement

You are given two tables: `Customers` and `Orders`.

### Table: Customers

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |

`id` is the primary key.  
Each row of this table indicates the ID and name of a customer.

### Table: Orders

| Column Name | Type |
|-------------|------|
| id          | int  |
| customerId  | int  |

`id` is the primary key.  
`customerId` is a foreign key referencing the `id` from the `Customers` table.  
Each row of this table indicates the ID of an order and the ID of the customer who ordered it.

## Objective

Write a solution to find customers who have never placed an order.

## SQL Query

```sql
SELECT 
  name AS Customers
FROM Customers 
WHERE id NOT IN (
  SELECT customerId FROM Orders
);
