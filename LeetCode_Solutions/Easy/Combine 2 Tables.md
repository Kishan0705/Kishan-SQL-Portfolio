# SQL Problem: Combine Two Tables

## Problem Statement

You are given two tables: `Person` and `Address`.

### Table: Person

| Column Name | Type    |
|-------------|---------|
| personId    | int     |
| lastName    | varchar |
| firstName   | varchar |

`personId` is the primary key.  
This table contains information about the ID of some persons and their first and last names.

### Table: Address

| Column Name | Type    |
|-------------|---------|
| addressId   | int     |
| personId    | int     |
| city        | varchar |
| state       | varchar |

`addressId` is the primary key.  
Each row of this table contains information about the city and state of one person with ID = `personId`.

## Objective

Write a solution to report the first name, last name, city, and state of each person in the `Person` table. If the address of a `personId` is not present in the `Address` table, report null instead.

Return the result table in any order.

## SQL Query

```sql
SELECT 
  p.firstName, 
  p.lastName, 
  a.city, 
  a.state
FROM Person p
LEFT JOIN Address a 
  ON p.personId = a.personId;

