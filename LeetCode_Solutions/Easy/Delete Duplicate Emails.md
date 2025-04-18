# SQL Problem: Delete Duplicate Emails

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

Write a solution to delete all duplicate emails, keeping only one unique email with the smallest id.

For SQL users, please note that you are supposed to write a `DELETE` statement and not a `SELECT` one.

## Input

### Person table:

| id | email            |
|----|------------------|
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |

## SQL Query

```sql
DELETE p1 
FROM Person p1
JOIN Person p2
  ON p1.email = p2.email 
  AND p1.id > p2.id;
