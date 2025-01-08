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


