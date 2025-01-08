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

🟢 [ INSERT ] 

- **Insert Data into the promotions table for all employees earning more than the average salary of their department. Avoid inserting duplicate records.**

```sql

INSERT INTO promotions (emp_id, emp_name, dept_name) 
SELECT 
    e.emp_id, e.emp_name, e.dept_name
FROM 
    employee e 
WHERE 
    e.salary > (
        SELECT AVG(e1.salary)
        FROM employee e1
        WHERE e1.dept_name = e.dept_name
    )
    AND NOT EXISTS (
        SELECT 1 
        FROM promotions p 
        WHERE p.emp_id = e.emp_id
    );


```


🟡 [ UPDATE ] 

- **Update the employee table to increase the salary of all employees working in the "IT" department by 15%. Only update employees who are in the employee_history table.**

```sql

UPDATE employee e
SET salary = salary + (salary * 0.15)
WHERE dept_name = 'IT'
AND emp_id IN (
    SELECT eh.emp_id
    FROM employee_history eh
    WHERE eh.dept_name = 'IT'
);

```

🟡 [ UPDATE ] 

- **Update the employee table to set each employee's salary to the maximum salary of their department. Only consider employees who are also in the employee_history table.**

```sql

UPDATE employee e
SET salary = (
    SELECT MAX(eh.salary)
    FROM employee_history eh
    WHERE eh.dept_name = e.dept_name
)
WHERE emp_id IN (
    SELECT emp_id
    FROM employee_history
);

```

🟡 [ UPDATE ] 

- **Give 10% Increment to All employees in Banglore Location based on the MAXIMUM salary earned by an employee in each dept. Only Consider employees in employee_history Table.**

```sql

UPDATE employee e 
set salary = ( select max(salary) + max(salary) * 0.1 ) as 
	             from employee_history eh 
	              where eh.dept_name = e.dept_name) 
where e.dept_name in ( select dept_name 
	                     from department 
	                     where location = 'Banglore' ) 
and e.emp_id in ( select emp_id from employee_history );

```













