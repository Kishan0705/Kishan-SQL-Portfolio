# Mastering SQL Subqueries 

A  collection of solved SQL queries focused on subqueries, categorized by subquery types. These queries are designed to showcase mastery of SQL concepts, specifically for interview preparation, and serve as part of my SQL GitHub portfolio.


### 📍 Table of Contents

- [Subqueries in the **WHERE** Clause](https://github.com/Kishan0705/Kishan-SQL-Portfolio/blob/main/The-Subquery-Collection/README.md#subqueries-in-the-where-clause)
- [Subqueries in the **FROM** Clause](https://github.com/Kishan0705/Kishan-SQL-Portfolio/blob/main/The-Subquery-Collection/README.md#subqueries-in-the-from-clause)
- Subqueries in the **SELECT** Clause
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

**Scenario:**
**A retail company wants to identify the top 3 products (based on revenue) in each category. The revenue for a product is calculated as Price * UnitsSold.**

**Task:**
**Write a query to fetch the ProductID, ProductName, CategoryID, and Revenue for the top 3 products by revenue in each category. Use subqueries effectively.**

```sql

SELECT 
    ProductID, 
    ProductName, 
    CategoryID, 
    Revenue
FROM (
    SELECT 
        p.ProductID,
        p.ProductName,
        p.CategoryID,
        SUM(p.Price * s.UnitsSold) AS Revenue,
        DENSE_RANK() OVER (
            PARTITION BY p.CategoryID 
            ORDER BY SUM(p.Price * s.UnitsSold) DESC
        ) AS Rnk
    FROM 
        Products p
    JOIN 
        Sales s
    ON 
        p.ProductID = s.ProductID
    GROUP BY 
        p.ProductID, p.ProductName, p.CategoryID
) ranked
WHERE Rnk <= 3
ORDER BY CategoryID ASC, Revenue DESC;

```

**Find the names of customers who have placed orders for products from more than 2 categories.**

```sql
SELECT 
    CustomerName 
FROM (
    SELECT 
        c.CustomerName,
        COUNT(DISTINCT p.CategoryID) AS No_of_Categories 
    FROM 
        Customers c 
    JOIN 
        Orders o ON c.CustomerID = o.CustomerID 
    JOIN 
        OrderDetails od ON od.OrderID = o.OrderID 
    JOIN 
        Products p ON p.ProductID = od.ProductID
    GROUP BY 
        c.CustomerName
    HAVING 
        COUNT(DISTINCT p.CategoryID) > 2
) AS customer_categories;

```


## Correlated Subqueries 

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


## Usage of NOT EXISTS and NOT IN

**Scenario:**
**You’re working for a Logistics Company, and you’re tasked to analyze delivery performance. Your company has the following tables:**

**Task:**
**Write a query to find Driver Names in regions where all deliveries assigned to drivers have a status of 'Delivered'.**

```sql

SELECT 
    d.DriverName,
    d.RegionID
FROM 
    Drivers d
WHERE 
    NOT EXISTS (
        SELECT 1 
        FROM Deliveries dil
        WHERE 
            dil.DriverID = d.DriverID 
            AND dil.Status <> 'Delivered'
    );


```


**Question: Identify Customers with No Orders in the Last 6 Months**


```sql

SELECT 
    c.CustomerName 
FROM 
    Customers c
JOIN 
    Orders o ON c.CustomerID = o.CustomerID 
WHERE 
    NOT EXISTS (
        SELECT 1
        FROM Orders o1
        WHERE 
            o1.CustomerID = c.CustomerID 
            AND o1.OrderDate >= CURRENT_DATE - INTERVAL '6 months'
    );


```

**Q : Find the names of customers who have not placed any orders in the last 3 months.**

```sql
SELECT 
    c.CustomerName 
FROM 
    Customers c 
WHERE 
    NOT EXISTS ( 
        SELECT 1 
        FROM Orders o1
        WHERE o1.CustomerID = c.CustomerID 
        AND o1.OrderDate >= CURRENT_DATE - INTERVAL '3 months'
    );

```

**Find the names of employees who have never supervised anyone.**

```sql
SELECT 
    e.EmployeeName 
FROM 
    Employees e 
WHERE 
    e.EmployeeID NOT IN (
        SELECT DISTINCT ManagerID 
        FROM Employees 
        WHERE ManagerID IS NOT NULL
    );

```

**Find the names of employees who are not managing anyone and are not assigned to any pending projects.**
```sql
SELECT 
    e.EmployeeName
FROM 
    Employees e 
JOIN 
    Projects p ON e.EmployeeID = p.EmployeeID 
WHERE 
    NOT EXISTS (
        SELECT 1 
        FROM Projects p1
        WHERE 
            p1.EmployeeID = e.EmployeeID 
            AND p1.Status = 'Completed'
    )
    AND e.ManagerID IS NULL;
```












