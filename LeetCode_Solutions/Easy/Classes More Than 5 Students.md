# SQL Problem: Classes More Than 5 Students

## Problem Statement

You are given a table: `Courses`.

### Table: Courses

| Column Name | Type    |
|-------------|---------|
| student     | varchar |
| class       | varchar |

`(student, class)` is the primary key.  
Each row of this table indicates the name of a student and the class in which they are enrolled.

## Objective

Write a solution to find all the **classes** that have **at least five students**.

## SQL Query

```sql
SELECT
  class
FROM Courses 
GROUP BY class 
HAVING COUNT(*) >= 5;
