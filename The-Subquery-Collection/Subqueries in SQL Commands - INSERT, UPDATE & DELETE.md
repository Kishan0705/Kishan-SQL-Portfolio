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
SET salary = (
    SELECT MAX(eh.salary) + MAX(eh.salary) * 0.1
    FROM employee_history eh
    WHERE eh.dept_name = e.dept_name
)
WHERE e.dept_name IN (
    SELECT d.dept_name
    FROM department d
    WHERE d.location = 'Banglore'
)
AND e.emp_id IN (
    SELECT eh.emp_id
    FROM employee_history eh
);

```

🟡 [ UPDATE ] 

- **Update the employee table to give a 20% salary increment to employees whose current salary is below the average salary of their department. Only update employees who are listed in the employee_history table.**

```sql

UPDATE employee e
SET salary = salary + (salary * 0.20)
WHERE salary < (
    SELECT AVG(e1.salary)
    FROM employee e1
    WHERE e1.dept_name = e.dept_name
)
AND emp_id IN (
    SELECT emp_id
    FROM employee_history
);

```

🟢 [ INSERT ] 

- **Insert all employees who are not in the promotions table. or each inserted employee, include their emp_id, emp_name, and the maximum salary of their department as their promotion salary.**

- Tables:
  
**employee: (emp_id, emp_name, dept_name, salary)**

**promotions: (emp_id, emp_name, promotion_salary)**

```sql

INSERT INTO promotions (emp_id, emp_name, promotion_salary)
SELECT 
    e.emp_id, e.emp_name, (
        SELECT MAX(e1.salary)
        FROM employee e1
        WHERE e1.dept_name = e.dept_name
    ) AS promotion_salary
FROM 
    employee e
WHERE 
    NOT EXISTS (
        SELECT 1 
        FROM promotions p 
        WHERE p.emp_id = e.emp_id
    );

```

🟡 [ UPDATE ] 

- **Update the employee table to set the salary of employees to 10% less than the maximum salary of their department. Only update employees who are currently earning above the average salary of their department.**

```sql
UPDATE employee e
SET salary = (
    SELECT MAX(e1.salary) * 0.90
    FROM employee e1
    WHERE e1.dept_name = e.dept_name
)
WHERE salary > (
    SELECT AVG(e1.salary)
    FROM employee e1
    WHERE e1.dept_name = e.dept_name
);

```

🟡 [ UPDATE ] 

- **Update the salaries of employees such that all employees earning below the average salary of their department get a 15% increment. Consider only employees who have been with the company for more than 5 years (use the employee_history table to determine tenure).** 

- Tables:

**employee: (emp_id, emp_name, dept_name, salary)**

**employee_history: (emp_id, emp_name, dept_name, salary, hire_date)**

```sql

UPDATE employee e
SET salary = salary + (salary * 0.15)
WHERE salary < (
    SELECT AVG(e1.salary)
    FROM employee e1
    WHERE e1.dept_name = e.dept_name
)
AND emp_id IN (
    SELECT eh.emp_id
    FROM employee_history eh
    WHERE eh.hire_date <= CURRENT_DATE - INTERVAL '5 years'
);

```

🟡 [ UPDATE ] 

- **Update the department location for all employees who have not received a promotion. Move them to the "General" department. The update should only affect employees who are currently in the employee_history table and have been with the company for more than 3 years.**

- Tables:

**employee: (emp_id, emp_name, dept_name, location)**

**promotions: (emp_id, promotion_date)**

**employee_history: (emp_id, emp_name, dept_name, hire_date)**

```sql

UPDATE employee_history eh
SET dept_name = 'General'
WHERE eh.hire_date <= CURRENT_DATE - INTERVAL '3 years' 
AND NOT EXISTS (
    SELECT 1 
    FROM promotions p 
    WHERE p.emp_id = eh.emp_id 
);

```

🟢 [ INSERT ] 

- **Insert all employees who have been in the company for more than 5 years and earn above the average salary of their department into the high_earners table. The high_earners table should contain the employee's ID, name, and current salary. Ensure no duplicate entries are added to the high_earners table.**

- Tables :

**employee: (emp_id, emp_name, dept_name, salary)**

**employee_history: (emp_id, emp_name, dept_name, hire_date)**

**high_earners: (emp_id, emp_name, salary)**

```sql

INSERT INTO high_earners (emp_id, emp_name, salary)
SELECT 
    e.emp_id, e.emp_name, e.salary
FROM employee e
JOIN employee_history eh ON e.emp_id = eh.emp_id
WHERE eh.hire_date <= CURRENT_DATE - INTERVAL '5 years' 
AND e.salary > (
    SELECT AVG(e1.salary)
    FROM employee e1
    WHERE e1.dept_name = e.dept_name 
)
AND NOT EXISTS (
    SELECT 1 
    FROM high_earners he
    WHERE he.emp_id = e.emp_id 
);

```

🔴 [ DELETE ] 

- **Delete all employees from the employee table who are not present in the employee_history table and whose salaries are below the average salary of their department.**

- Tables :

**employee: (emp_id, emp_name, dept_name, salary)**

**employee_history: (emp_id, emp_name, dept_name, hire_date)**

```sql

DELETE FROM employee e
WHERE e.salary < (
    SELECT AVG(e1.salary)
    FROM employee e1
    WHERE e1.dept_name = e.dept_name 
)
AND NOT EXISTS (
    SELECT 1
    FROM employee_history eh
    WHERE eh.emp_id = e.emp_id 
);

```

🔴 [ DELETE ] 

- **Delete all departments from the department table that do not have any employees associated with them in the employee table.**

**Tables:**

- **department: (dept_id, dept_name, location)**
- **employee: (emp_id, emp_name, dept_name, salary)**

```sql

DELETE FROM department
WHERE dept_name IN (
    SELECT d.dept_name
    FROM department d
    WHERE NOT EXISTS (
        SELECT 1
        FROM employee e
        WHERE e.dept_name = d.dept_name -- No employees in this department
    )
);

```

















