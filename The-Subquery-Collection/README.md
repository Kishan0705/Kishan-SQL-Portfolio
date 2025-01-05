# Mastering SQL Subqueries 

A  collection of solved SQL queries focused on subqueries, categorized by subquery types. These queries are designed to showcase mastery of SQL concepts, specifically for interview preparation, and serve as part of my SQL GitHub portfolio.


### 📍 Table of Contents

- Subqueries in the **WHERE** Clause
- Subqueries in the **SELECT** Clause
- Subqueries in the **FROM** Clause
- **Correlated Subqueries**
- Usage of **NOT EXISTS** and **NOT IN**
- Miscellaneous **Subquery Use Cases**


## Subqueries in the WHERE Clause

-  **Select all employees and salary who are earning more than employee 7839**
```sql
SELECT ename, sal
FROM emp
WHERE sal > (SELECT sal FROM emp WHERE empno = '7839')
ORDER BY sal DESC;
```

- **Select all details of employees who belong to the same department as employee 7839**
```sql

SELECT * 
FROM emp e 
WHERE e.deptno = (
    SELECT e1.deptno 
    FROM emp e1 
    WHERE e1.empno = '7839'
);

```

-  **Select all employee details who were hired before BLAKE**
```sql

SELECT * 
FROM emp e 
WHERE e.hiredate < (SELECT hiredate FROM emp WHERE ename = 'BLAKE');

```

- **Select all employee names and salary who are earning more than BLAKE but less than KING**

```sql

SELECT 
    e.ename,
    e.sal
FROM 
    emp e
WHERE 
    e.sal > (SELECT sal FROM emp WHERE ename = 'BLAKE')
    AND e.sal < (SELECT sal FROM emp WHERE ename = 'KING');

```

- **Display 3rd highest salary using subquery (without ranking functions)**

```sql

SELECT MAX(sal) AS third_highest_salary
FROM emp
WHERE sal < (
    SELECT MAX(sal)
    FROM emp
    WHERE sal < (
        SELECT MAX(sal)
        FROM emp
    )
);

```
