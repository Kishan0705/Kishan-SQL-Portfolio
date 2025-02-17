# [ 14 ]

- We define an employee's total earnings to be their monthly Salary * Months worked, and the maximum total earnings to be the maximum total earnings for any employee in the Employee table. Write a query to find the maximum total earnings for all employees as well as the total number of employees who have maximum total earnings. Then print these values as 2 space-separated integers.

```sql

SELECT 
    MAX(Salary * Months) AS Max_Total_Earnings,
    COUNT(*) AS Number_of_Employees_With_Max_Earnings
FROM Employee
WHERE Salary * Months = (SELECT MAX(Salary * Months) FROM Employee);

```
