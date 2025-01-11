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

- **A retail company wants to identify the products with a price higher than the average price of all products in the same category. Write a query to retrieve the product name, category, and price of such products.**
```sql

SELECT 
    p.ProductName,
    p.CategoryID,
    p.Price
FROM 
    Products p
WHERE 
    p.Price > (
        SELECT 
            AVG(p1.Price) 
        FROM 
            Products p1
        WHERE 
            p1.CategoryID = p.CategoryID
    );

```

- **Find Employees Earning Above Their Department's Average Salary**
```sql

SELECT 
    e.EmployeeName,
    e.Salary,
    e.DepartmentID
FROM Employees e 
WHERE e.Salary > 
    ( SELECT AVG(e1.Salary) 
      FROM Employees e1 
      WHERE e1.DepartmentID = e.DepartmentID );

```

- **Calculate the total salary for each department while excluding the top earner in that department.**

**Table:**

- **employee: (emp_id, emp_name, dept_name, salary)**

```sql

SELECT dept_name, SUM(salary) AS total_salary_excluding_top
FROM employee e
WHERE emp_id NOT IN (
    SELECT emp_id
    FROM employee e1
    WHERE e1.salary = (
        SELECT MAX(salary)
        FROM employee e2
        WHERE e2.dept_name = e1.dept_name
    )
)
GROUP BY dept_name;

```





