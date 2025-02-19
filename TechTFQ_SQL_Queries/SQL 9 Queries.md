# SQL Queries(Practice Complex SQL Queries)

## I have listed below 9 SQL Queries which should help you to practice intermediate to complex SQL queries.  

## 1️⃣ Write a SQL Query to fetch all the duplicate records in a table.

Note: Record is considered duplicate if a user name is present more than once.

👉 **Approach: Partition the data based on user name and then give a row number to each of the partitioned user name. If a user name exists more than once then it would have multiple row numbers. Using the row number which is other than 1, we can identify the duplicate records.**

*Table Name: USERS*

![image](https://github.com/user-attachments/assets/7e86d911-a359-47f7-b302-674bd8d446e2)

***➜ Solution***

💡 **Approach No: 1 [ Using CTEs ]**

```sql
with cte as ( 
select 
*,
row_number() over (partition by user_name order by user_id) as numbers 
from users 
order by user_id)
select 
user_id, user_name, email 
from cte 
where numbers >1;
```

💡 **Approach No: 2 [ Using Subquery ]**
```sql
select user_id, user_name, email
from (
select *,
row_number() over (partition by user_name order by user_id) as rn
from users u
order by user_id) x
where x.rn <> 1;
```

## 2️⃣ Write a SQL query to fetch the second last record from employee table.

👉 **Approach: Using window function sort the data in descending order based on employee id. Provide a row number to each of the record and fetch the record having row number as 2.**

*Table Name: EMPLOYEE*

![image](https://github.com/user-attachments/assets/20bc8c08-7f43-4172-8569-4ce97a2e575b)

***➜ Solution***

```sql
select emp_id, emp_name, dept_name, salary
from (
select *,
row_number() over (order by emp_id desc) as rn
from employee e) x
where x.rn = 2;
```







