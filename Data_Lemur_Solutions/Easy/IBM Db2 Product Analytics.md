## Problem Statement: IBM Db2 Product Analytics

**IBM SQL Interview Question**

IBM is analyzing how their employees are utilizing the Db2 database by tracking the SQL queries executed by their employees. The objective is to generate data to populate a histogram that shows the number of unique queries run by employees during the third quarter of 2023 (July to September). Additionally, it should count the number of employees who did not run any queries during this period.

**Display the number of unique queries as histogram categories, along with the count of employees who executed that number of unique queries.**

### SQL Query:

```sql
WITH cte AS (
    SELECT
        e.employee_id,
        COUNT(DISTINCT q.query_id) AS no_of_queries
    FROM employees e
    LEFT JOIN queries q  -- Ensure all employees are included
        ON e.employee_id = q.employee_id
        AND q.query_starttime >= '2023-07-01 00:00:00'
        AND q.query_starttime < '2023-10-01 00:00:00'
    GROUP BY e.employee_id
)
SELECT
    no_of_queries AS unique_queries,
    COUNT(employee_id) AS employee_count
FROM cte
GROUP BY unique_queries
ORDER BY unique_queries;
```

