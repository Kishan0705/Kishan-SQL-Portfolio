# Mastering SQL Subqueries 

A  collection of solved SQL queries focused on subqueries, categorized by subquery types. These queries are designed to showcase mastery of SQL concepts, specifically for interview preparation, and serve as part of my SQL GitHub portfolio.


### 📍 Table of Contents

- [Subqueries in the **WHERE** Clause](https://github.com/Kishan0705/Kishan-SQL-Portfolio/blob/main/The-Subquery-Collection/README.md#subqueries-in-the-where-clause)
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


## Subqueries in the FROM Clause

- **Select all department numbers which have more employees than department 10**
```sql

SELECT 
    deptno
FROM (
    SELECT 
        deptno,
        COUNT(empno) AS Total_Employees
    FROM 
        emp
    GROUP BY 
        deptno
    HAVING 
        COUNT(empno) > (
            SELECT COUNT(empno) 
            FROM emp 
            WHERE deptno = '10'
        )
) AS dept_count;

```
**Alternative Solution**

```sql
SELECT deptno
FROM (
    SELECT deptno, COUNT(empno) AS Total_Employees
    FROM emp
    GROUP BY deptno
) dept_counts
WHERE Total_Employees > (
    SELECT COUNT(empno)
    FROM emp
    WHERE deptno = '10'
);

```

 - **Select all department names which have more employees working than 'ACCOUNTING' department**
```sql

SELECT 
    dname
FROM (
    SELECT 
        d.dname,
        COUNT(e.empno) AS Total_Employees
    FROM 
        dept d 
    LEFT JOIN 
        emp e 
    ON 
        d.deptno = e.deptno
    GROUP BY 
        d.dname
    HAVING 
        COUNT(e.empno) > (
            SELECT COUNT(empno) 
            FROM emp 
            WHERE deptno = '10'
        )
) AS dept_employee_count;

```

