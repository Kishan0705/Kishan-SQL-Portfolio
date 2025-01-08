## Subqueries in SQL Commands - INSERT, UPDATE & DELETE

🟢 [ INSERT ] 

- **Insert Data into the employee_history table. Make sure no employee is inserted if they already exist in the history. Only insert employees who are in the "Sales" department.**

```sql

INSERT INTO employee_history (emp_id, emp_name, dept_name, salary)
SELECT 
    e.emp_id, e.emp_name, e.dept_name, e.salary
FROM 
    employee e
WHERE 
    e.dept_name = 'Sales'
    AND NOT EXISTS (
        SELECT 1 
        FROM employee_history eh 
        WHERE eh.emp_id = e.emp_id
    );

```

