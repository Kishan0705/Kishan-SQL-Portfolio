
## Subqueries in the SELECT Clause

- **List each employee’s details along with their department’s average salary.**

**Table :**

- **employee: (emp_id, emp_name, dept_name, salary)**

```sql

SELECT 
    emp_id, 
    emp_name, 
    dept_name, 
    salary,
    (SELECT AVG(e1.salary) 
     FROM employee e1 
     WHERE e1.dept_name = e.dept_name) AS dept_avg_salary
FROM employee e;

```

- **Show each employee’s rank in their department based on salary.**

**Table :**

- **employee: (emp_id, emp_name, dept_name, salary)**

```sql

SELECT 
    emp_id, 
    emp_name, 
    dept_name, 
    salary,
    (SELECT COUNT(*) + 1 
     FROM employee e1 
     WHERE e1.dept_name = e.dept_name 
       AND e1.salary > e.salary) AS rank_in_department
FROM employee e;

```

- **Add a column indicating whether an employee’s salary is above or below the company-wide average salary.**

**Table :**

- **employee: (emp_id, emp_name, dept_name, salary)**

```sql

SELECT 
    emp_id, 
    emp_name, 
    dept_name, 
    salary,
    CASE 
        WHEN salary > (SELECT AVG(salary) FROM employee) THEN 'Above Average'
        ELSE 'Below Average'
    END AS salary_category
FROM employee;

```

- **For each employee, calculate the percentage of their salary in relation to their department’s total salary.**

**Table :**

- **employee: (emp_id, emp_name, dept_name, salary)**

```sql

SELECT 
    emp_id, 
    emp_name, 
    dept_name, 
    salary,
    ROUND((salary / (SELECT SUM(e1.salary) 
                     FROM employee e1 
                     WHERE e1.dept_name = e.dept_name)) * 100, 2) AS salary_percentage
FROM employee e;

```

- **Show each employee’s details along with the maximum salary in their department.**

**Table :**

- **employee: (emp_id, emp_name, dept_name, salary)**

```sql

SELECT 
    emp_id, 
    emp_name, 
    dept_name, 
    salary,
    (SELECT MAX(e1.salary) 
     FROM employee e1 
     WHERE e1.dept_name = e.dept_name) AS max_salary_in_dept
FROM employee e;

```
























