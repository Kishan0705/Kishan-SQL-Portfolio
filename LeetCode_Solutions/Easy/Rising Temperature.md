# SQL Problem: Rising Temperature

## Problem Statement

You are given a table: `Weather`.

### Table: Weather

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| recordDate  | date    |
| temperature | int     |

`id` is the primary key.  
Each row of this table contains the temperature record of a specific day.

## Objective

Write a solution to find all dates' `id` with higher temperatures compared to its previous dates (yesterday).

## Input

### Weather table:

| id | recordDate | temperature |
|----|------------|-------------|
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |

## SQL Query

```sql
WITH cte AS (
  SELECT
    id, 
    temperature, 
    LAG(temperature) OVER () AS Prev_tem 
  FROM Weather
) 
SELECT 
  id 
FROM cte 
WHERE temperature > Prev_tem;
