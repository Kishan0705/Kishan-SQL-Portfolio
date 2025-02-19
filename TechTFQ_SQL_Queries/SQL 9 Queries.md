# SQL Queries(Practice Complex SQL Queries)

## I have listed below 9 SQL Queries which should help you to practice intermediate to complex SQL queries.  

## [ 1 ]  Write a SQL Query to fetch all the duplicate records in a table.

Note: Record is considered duplicate if a user name is present more than once.

👉 **Approach: Partition the data based on user name and then give a row number to each of the partitioned user name. If a user name exists more than once then it would have multiple row numbers. Using the row number which is other than 1, we can identify the duplicate records.**

*Table Name: USERS*

```sql
drop table users;
create table users
(
user_id int primary key,
user_name varchar(30) not null,
email varchar(50));

insert into users values
(1, 'Sumit', 'sumit@gmail.com'),
(2, 'Reshma', 'reshma@gmail.com'),
(3, 'Farhana', 'farhana@gmail.com'),
(4, 'Robin', 'robin@gmail.com'),
(5, 'Robin', 'robin@gmail.com');
```

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

## [ 2 ]  Write a SQL query to fetch the second last record from employee table.

👉 **Approach: Using window function sort the data in descending order based on employee id. Provide a row number to each of the record and fetch the record having row number as 2.**

*Table Name: EMPLOYEE*

```sql
--Tables Structure:

drop table employee;
create table employee
( emp_ID int primary key
, emp_NAME varchar(50) not null
, DEPT_NAME varchar(50)
, SALARY int);

insert into employee values(101, 'Mohan', 'Admin', 4000);
insert into employee values(102, 'Rajkumar', 'HR', 3000);
insert into employee values(103, 'Akbar', 'IT', 4000);
insert into employee values(104, 'Dorvin', 'Finance', 6500);
insert into employee values(105, 'Rohit', 'HR', 3000);
insert into employee values(106, 'Rajesh',  'Finance', 5000);
insert into employee values(107, 'Preet', 'HR', 7000);
insert into employee values(108, 'Maryam', 'Admin', 4000);
insert into employee values(109, 'Sanjay', 'IT', 6500);
insert into employee values(110, 'Vasudha', 'IT', 7000);
insert into employee values(111, 'Melinda', 'IT', 8000);
insert into employee values(112, 'Komal', 'IT', 10000);
insert into employee values(113, 'Gautham', 'Admin', 2000);
insert into employee values(114, 'Manisha', 'HR', 3000);
insert into employee values(115, 'Chandni', 'IT', 4500);
insert into employee values(116, 'Satya', 'Finance', 6500);
insert into employee values(117, 'Adarsh', 'HR', 3500);
insert into employee values(118, 'Tejaswi', 'Finance', 5500);
insert into employee values(119, 'Cory', 'HR', 8000);
insert into employee values(120, 'Monica', 'Admin', 5000);
insert into employee values(121, 'Rosalin', 'IT', 6000);
insert into employee values(122, 'Ibrahim', 'IT', 8000);
insert into employee values(123, 'Vikram', 'IT', 8000);
insert into employee values(124, 'Dheeraj', 'IT', 11000);
```
***➜ Solution***

```sql
select emp_id, emp_name, dept_name, salary
from (
select *,
row_number() over (order by emp_id desc) as rn
from employee e) x
where x.rn = 2;
```

## [ 3 ] Write a SQL query to display only the details of employees who either earn the highest salary or the lowest salary in each department from the employee table.

👉 **Approach: Write a sub query which will partition the data based on each department and then identify the record with maximum and minimum salary for each of the partitioned department. Finally, from the main query fetch only the data which matches the maximum and minimum salary returned from the sub query.**

```sql

drop table employee;
create table employee
( emp_ID int primary key
, emp_NAME varchar(50) not null
, DEPT_NAME varchar(50)
, SALARY int);

insert into employee values(101, 'Mohan', 'Admin', 4000);
insert into employee values(102, 'Rajkumar', 'HR', 3000);
insert into employee values(103, 'Akbar', 'IT', 4000);
insert into employee values(104, 'Dorvin', 'Finance', 6500);
insert into employee values(105, 'Rohit', 'HR', 3000);
insert into employee values(106, 'Rajesh',  'Finance', 5000);
insert into employee values(107, 'Preet', 'HR', 7000);
insert into employee values(108, 'Maryam', 'Admin', 4000);
insert into employee values(109, 'Sanjay', 'IT', 6500);
insert into employee values(110, 'Vasudha', 'IT', 7000);
insert into employee values(111, 'Melinda', 'IT', 8000);
insert into employee values(112, 'Komal', 'IT', 10000);
insert into employee values(113, 'Gautham', 'Admin', 2000);
insert into employee values(114, 'Manisha', 'HR', 3000);
insert into employee values(115, 'Chandni', 'IT', 4500);
insert into employee values(116, 'Satya', 'Finance', 6500);
insert into employee values(117, 'Adarsh', 'HR', 3500);
insert into employee values(118, 'Tejaswi', 'Finance', 5500);
insert into employee values(119, 'Cory', 'HR', 8000);
insert into employee values(120, 'Monica', 'Admin', 5000);
insert into employee values(121, 'Rosalin', 'IT', 6000);
insert into employee values(122, 'Ibrahim', 'IT', 8000);
insert into employee values(123, 'Vikram', 'IT', 8000);
insert into employee values(124, 'Dheeraj', 'IT', 11000);
```

***➜ Solution***

```sql

-- 1 Way ( using JOIN Subquery)

SELECT x.*  
FROM employee e  
JOIN (  
    SELECT *,  
           MAX(salary) OVER (PARTITION BY dept_name) AS max_salary,  
           MIN(salary) OVER (PARTITION BY dept_name) AS min_salary  
    FROM employee  
) x  
ON e.emp_id = x.emp_id  
AND (e.salary = x.max_salary OR e.salary = x.min_salary)  
ORDER BY x.dept_name, x.salary;  

-- 2 Way ( Usinf CTE )

WITH cte AS (
    SELECT *,
           MIN(salary) OVER (PARTITION BY dept_name) AS min_sal,
           MAX(salary) OVER (PARTITION BY dept_name) AS max_sal
    FROM employee
)
SELECT emp_id, emp_name, dept_name, salary
FROM cte
WHERE salary = min_sal OR salary = max_sal
ORDER BY dept_name, salary;

```

## [ 4 ] From the doctors table, fetch the details of doctors who work in the same hospital but in different specialty.

**👉 Approach: Use self join to solve this problem. Self join is when you join a table to itself.**

**Additional Query: Write SQL query to fetch the doctors who work in same hospital irrespective of their specialty.**

```sql
--Table Structure:

drop table doctors;
create table doctors
(
id int primary key,
name varchar(50) not null,
speciality varchar(100),
hospital varchar(50),
city varchar(50),
consultation_fee int
);

insert into doctors values
(1, 'Dr. Shashank', 'Ayurveda', 'Apollo Hospital', 'Bangalore', 2500),
(2, 'Dr. Abdul', 'Homeopathy', 'Fortis Hospital', 'Bangalore', 2000),
(3, 'Dr. Shwetha', 'Homeopathy', 'KMC Hospital', 'Manipal', 1000),
(4, 'Dr. Murphy', 'Dermatology', 'KMC Hospital', 'Manipal', 1500),
(5, 'Dr. Farhana', 'Physician', 'Gleneagles Hospital', 'Bangalore', 1700),
(6, 'Dr. Maryam', 'Physician', 'Gleneagles Hospital', 'Bangalore', 1500);
```
***➜ Solution***

```sql

select d1.name, d1.speciality,d1.hospital
from doctors d1
join doctors d2
on d1.hospital = d2.hospital and d1.speciality <> d2.speciality
and d1.id <> d2.id;

-- Alternative Question solution 

select d1.name, d1.speciality,d1.hospital
from doctors d1
join doctors d2
on d1.hospital = d2.hospital
and d1.id <> d2.id;

```

## [ 5 ] From the login_details table, fetch the users who logged in consecutively 3 or more times.

**👉 We need to fetch users who have appeared 3 or more times consecutively in login details table. There is a window function which can be used to fetch data from the following record. Use that window function to compare the user name in current row with user name in the next row and in the row following the next row. If it matches then fetch those records.**

```sql
--Table Structure:

drop table login_details;
create table login_details(
login_id int primary key,
user_name varchar(50) not null,
login_date date);

delete from login_details;
insert into login_details values
(101, 'Michael', current_date),
(102, 'James', current_date),
(103, 'Stewart', current_date+1),
(104, 'Stewart', current_date+1),
(105, 'Stewart', current_date+1),
(106, 'Michael', current_date+2),
(107, 'Michael', current_date+2),
(108, 'Stewart', current_date+3),
(109, 'Stewart', current_date+3),
(110, 'James', current_date+4),
(111, 'James', current_date+4),
(112, 'James', current_date+5),
(113, 'James', current_date+6);
```

***➜ Solution***

```sql
select 
	distinct X.repeatitive_name
	from ( 
select 
*,
case 
    when user_name = lead(user_name) over ( order by login_id ) AND
     user_name = lead(user_name,2) over ( order by login_id )
then user_name 
else NULL
end as repeatitive_name
from login_details ) X
where X.repeatitive_name is not null;

```

## [ 6 ] From the students table, write a SQL query to interchange the adjacent student names.

**👉Assuming id will be a sequential number always. If id is an odd number then fetch the student name from the following record. If id is an even number then fetch the student name from the preceding record. Try to figure out the window function which can be used to fetch the preceding the following record data.**

**If the last record is an odd number then it wont have any adjacent even number hence figure out a way to not interchange the last record data.**

```sql
--Table Structure:

drop table students;
create table students
(
id int primary key,
student_name varchar(50) not null
);
insert into students values
(1, 'James'),
(2, 'Michael'),
(3, 'George'),
(4, 'Stewart'),
(5, 'Robin');
```

***➜ Solution***

```sql
SELECT 
    id,
    student_name,
    CASE 
        WHEN id % 2 <> 0 
            THEN LEAD(student_name, 1, student_name) OVER (ORDER BY id)
        WHEN id % 2 = 0 
            THEN LAG(student_name) OVER (ORDER BY id)
    END AS new_student_name
FROM students;
```

## [ 7 ] From the weather table, fetch all the records when London had extremely cold temperature for 3 consecutive days or more.

**👉First using a sub query identify all the records where the temperature was very cold and then use a main query to fetch only the records returned as very cold from the sub query. You will not only need to compare the records following the current row but also need to compare the records preceding the current row. And may also need to compare rows preceding and following the current row. Identify a window function which can do this comparison pretty easily.**

```sql
--Table Structure:

drop table weather;
create table weather
(
id int,
city varchar(50),
temperature int,
day date
);
delete from weather;
insert into weather values
(1, 'London', -1, to_date('2021-01-01','yyyy-mm-dd')),
(2, 'London', -2, to_date('2021-01-02','yyyy-mm-dd')),
(3, 'London', 4, to_date('2021-01-03','yyyy-mm-dd')),
(4, 'London', 1, to_date('2021-01-04','yyyy-mm-dd')),
(5, 'London', -2, to_date('2021-01-05','yyyy-mm-dd')),
(6, 'London', -5, to_date('2021-01-06','yyyy-mm-dd')),
(7, 'London', -7, to_date('2021-01-07','yyyy-mm-dd')),
(8, 'London', 5, to_date('2021-01-08','yyyy-mm-dd'));

```

***➜ Solution***

```sql
SELECT id, city, temperature, day
FROM (
    SELECT *,
        CASE 
            WHEN temperature < 0 
                 AND LEAD(temperature) OVER (ORDER BY day) < 0
                 AND LEAD(temperature, 2) OVER (ORDER BY day) < 0 
            THEN 'Y'
            
            WHEN temperature < 0 
                 AND LEAD(temperature) OVER (ORDER BY day) < 0
                 AND LAG(temperature) OVER (ORDER BY day) < 0 
            THEN 'Y'
            
            WHEN temperature < 0 
                 AND LAG(temperature) OVER (ORDER BY day) < 0
                 AND LAG(temperature, 2) OVER (ORDER BY day) < 0 
            THEN 'Y'
            
            ELSE NULL 
        END AS flag
    FROM weather
) x
WHERE x.flag = 'Y'
ORDER BY day;
```


## [ 8 ] From the following 3 tables (event_category, physician_speciality, patient_treatment), write a SQL query to get the histogram of specialties of the unique physicians who have done the procedures but never did prescribe anything.

**👉Using the patient treatment and event category table, identify all the physicians who have done “Prescription”. Have this recorded in a sub query.**

**Then in the main query join the patient treatment, event category and physician speciality table to identify all the physician who have done “Procedure”. From these physicians, remove those physicians you got from sub query to return the physicians who have done Procedure but never did Prescription.**

```sql
--Table Structure:

drop table event_category;
create table event_category
(
  event_name varchar(50),
  category varchar(100)
);

drop table physician_speciality;
create table physician_speciality
(
  physician_id int,
  speciality varchar(50)
);

drop table patient_treatment;
create table patient_treatment
(
  patient_id int,
  event_name varchar(50),
  physician_id int
);


insert into event_category values ('Chemotherapy','Procedure');
insert into event_category values ('Radiation','Procedure');
insert into event_category values ('Immunosuppressants','Prescription');
insert into event_category values ('BTKI','Prescription');
insert into event_category values ('Biopsy','Test');


insert into physician_speciality values (1000,'Radiologist');
insert into physician_speciality values (2000,'Oncologist');
insert into physician_speciality values (3000,'Hermatologist');
insert into physician_speciality values (4000,'Oncologist');
insert into physician_speciality values (5000,'Pathologist');
insert into physician_speciality values (6000,'Oncologist');


insert into patient_treatment values (1,'Radiation', 1000);
insert into patient_treatment values (2,'Chemotherapy', 2000);
insert into patient_treatment values (1,'Biopsy', 1000);
insert into patient_treatment values (3,'Immunosuppressants', 2000);
insert into patient_treatment values (4,'BTKI', 3000);
insert into patient_treatment values (5,'Radiation', 4000);
insert into patient_treatment values (4,'Chemotherapy', 2000);
insert into patient_treatment values (1,'Biopsy', 5000);
insert into patient_treatment values (6,'Chemotherapy', 6000);
```

***➜ Solution***

```sql

SELECT 
    ps.speciality,
    COUNT(*) AS speciality_count
FROM patient_treatment pt
JOIN event_category e 
    ON pt.event_name = e.event_name
JOIN physician_speciality ps 
    ON ps.physician_id = pt.physician_id
WHERE e.category = 'Procedure'
AND pt.physician_id NOT IN ( 
    SELECT pt2.physician_id
    FROM patient_treatment pt2
    JOIN event_category e2 
        ON pt2.event_name = e2.event_name
    WHERE e2.category = 'Prescription'
)
GROUP BY ps.speciality;

```










