# SQL Problem: Employees Earning More Than Their Managers

## Problem Statement

You are given a table: `Employee`.

### Table: Employee

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
| salary      | int     |
| managerId   | int     |

`id` is the primary key.  
Each row of this table indicates the ID of an employee, their name, salary, and the ID of their manager.

## Objective

Write a solution to find the employees who earn more than their managers.

## SQL Query

```sql
SELECT
  e.name AS Employee
FROM Employee e 
JOIN Employee m 
  ON e.managerId = m.id 
WHERE e.salary > m.salary;
