# SQL Problem: Customers Placing the Largest Number of Orders

## Problem Statement

You are given a table: `Orders`.

### Table: Orders

| Column Name     | Type |
|------------------|------|
| order_number     | int  |
| customer_number  | int  |

`order_number` is the primary key.  
Each row of this table contains the order number and the customer number of the customer who placed the order.

## Objective

Write a solution to find the `customer_number` for the customer who has placed the **largest number of orders**.

## Input

### Orders table:

| order_number | customer_number |
|--------------|-----------------|
| 1            | 1               |
| 2            | 2               |
| 3            | 3               |
| 4            | 3               |

## SQL Query

```sql
SELECT 
  customer_number 
FROM Orders 
GROUP BY customer_number
ORDER BY COUNT(order_number) DESC 
LIMIT 1;
