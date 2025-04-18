# SQL Problem: Duplicate Email

## Problem Statement

You are given a table: `Person`.

### Table: Person

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| email       | varchar |

`id` is the primary key.  
Each row of this table contains an email. The emails will not contain uppercase letters.

## Objective

Write a solution to report all the duplicate emails.  
Note that it's guaranteed that the `email` field is not NULL.

## SQL Query

```sql
SELECT 
  email AS Email 
FROM (
  SELECT 
    email, COUNT(id) AS cnt 
  FROM Person 
  GROUP BY email 
  HAVING COUNT(id) > 1
) x;
