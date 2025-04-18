# SQL Problem: Find Customer Referee

## Problem Statement

You are given a table: `Customer`.

### Table: Customer

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
| referee_id  | int     |

`id` is the primary key.  
Each row of this table contains information about a customer and the ID of the customer who referred them.

## Objective

Find the names of the customers that are **not referred by the customer with id = 2**.

## Input

### Customer table:

| id | name | referee_id |
|----|------|------------|
| 1  | Will | null       |
| 2  | Jane | null       |
| 3  | Alex | 2          |
| 4  | Bill | null       |
| 5  | Zack | 1          |
| 6  | Mark | 2          |

## SQL Query

```sql
SELECT name 
FROM Customer 
WHERE referee_id <> 2 
   OR referee_id IS NULL;
