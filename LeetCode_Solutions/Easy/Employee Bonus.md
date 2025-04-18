# SQL Problem: Employee Bonus

## Problem Statement

You are given two tables: `Employee` and `Bonus`.

### Table: Employee

| Column Name | Type    |
|-------------|---------|
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |

`empId` is the primary key.  
Each row of this table contains information about an employee.

### Table: Bonus

| Column Name | Type |
|-------------|------|
| empId       | int  |
| bonus       | int  |

`empId` is the primary key.  
Each row of this table contains the bonus amount for an employee.

## Objective

Write a solution to report the name and bonus amount of each employee with a bonus less than 1000.

## Input

### Employee table:

| empId | name   | supervisor | salary |
|-------|--------|------------|--------|
| 3     | Brad   | null       | 4000   |
| 1     | John   | 3          | 1000   |
| 2     | Dan    | 3          | 2000   |
| 4     | Thomas | 3          | 4000   |

### Bonus table:

| empId | bonus |
|-------|-------|
| 2     | 500   |
| 4     | 2000  |

## SQL Query

```sql
SELECT 
  e.name, 
  b.bonus
FROM Employee e
LEFT JOIN Bonus b 
  ON e.empId = b.empId
WHERE b.bonus < 1000 OR b.bonus IS NULL;
