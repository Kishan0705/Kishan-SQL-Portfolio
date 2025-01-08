# Mastering SQL Subqueries 

A  collection of solved SQL queries focused on subqueries, categorized by subquery types. These queries are designed to showcase mastery of SQL concepts, specifically for interview preparation, and serve as part of my SQL GitHub portfolio.


### 📍 Table of Contents

- [Subqueries in the **WHERE** Clause](https://github.com/Kishan0705/Kishan-SQL-Portfolio/blob/main/The-Subquery-Collection/Subqueries%20in%20the%20WHERE%20Clause.md#subqueries-in-the-where-clause)
- [Subqueries in the **FROM** Clause](https://github.com/Kishan0705/Kishan-SQL-Portfolio/blob/main/The-Subquery-Collection/Subqueries%20in%20the%20FROM%20Clause.md#subqueries-in-the-from-clause)
- Subqueries in the **SELECT** Clause
- [**Correlated Subqueries**](https://github.com/Kishan0705/Kishan-SQL-Portfolio/blob/main/The-Subquery-Collection/Correlated%20Subqueries.md#correlated-subqueries)
- [Usage of **NOT EXISTS** and **NOT IN**](https://github.com/Kishan0705/Kishan-SQL-Portfolio/blob/main/The-Subquery-Collection/README.md#usage-of-not-exists-and-not-in)
- [Subqueries in SQL Commands - INSERT, UPDATE & DELETE]



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












