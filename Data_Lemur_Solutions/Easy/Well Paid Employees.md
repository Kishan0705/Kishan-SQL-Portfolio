## Problem Statement:  

### Well Paid Employees  
**FAANG SQL Interview Question**  

Companies often perform salary analyses to ensure fair compensation practices. One useful analysis is to check if there are any employees earning more than their direct managers.

As an HR Analyst, you're asked to identify all employees who earn more than their direct managers. The result should include the employee's ID and name.

---

### SQL Query:
```sql
SELECT 
    e.employee_id,
    e.name
FROM employee e
JOIN employee m
ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
