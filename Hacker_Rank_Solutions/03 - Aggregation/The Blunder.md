# [ 13 ] 

- Samantha was tasked with calculating the average monthly salaries for all employees in the EMPLOYEES table, but did not realize her keyboard's  0  key was broken until after completing the calculation. She wants your help finding the difference between her miscalculation (using salaries with any zeros removed), and the actual average salary.

Write a query calculating the amount of error (i.e.: Actual - Miscalculated  average monthly salaries), and round it up to the next integer.

```sql

SELECT 
    CEIL(
        ABS(AVG(Salary) - AVG(Cleaned_Salary))
    ) AS Error_Amount
FROM (
    SELECT 
        Salary, 
        CAST(REPLACE(CAST(Salary AS CHAR), '0', '') AS UNSIGNED) AS Cleaned_Salary
    FROM EMPLOYEES
) AS CleanedSalaries;

```
