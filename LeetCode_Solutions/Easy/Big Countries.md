# SQL Problem: Big Countries

## Problem Statement

You are given a table: `World`.

### Table: World

| Column Name | Type    |
|-------------|---------|
| name        | varchar |
| continent   | varchar |
| area        | int     |
| population  | int     |
| gdp         | bigint  |

`name` is the primary key.  
Each row of this table contains information about a country.

## Objective

A country is considered **big** if:

- It has an area of at least 3,000,000 (`area >= 3000000`), **or**
- It has a population of at least 25,000,000 (`population >= 25000000`)

Write a solution to find the `name`, `population`, and `area` of the big countries.

## Input

### World table:

| name        | continent | area    | population | gdp          |
|-------------|-----------|---------|------------|--------------|
| Afghanistan | Asia      | 652230  | 25500100   | 20343000000  |
| Albania     | Europe    | 28748   | 2831741    | 12960000000  |
| Algeria     | Africa    | 2381741 | 37100000   | 188681000000 |
| Andorra     | Europe    | 468     | 78115      | 3712000000   |
| Angola      | Africa    | 1246700 | 20609294   | 100990000000 |

## SQL Query

```sql
SELECT 
  name, 
  population, 
  area 
FROM World 
WHERE area >= 3000000 
   OR population >= 25000000;
