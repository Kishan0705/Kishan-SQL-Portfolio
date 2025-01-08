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


