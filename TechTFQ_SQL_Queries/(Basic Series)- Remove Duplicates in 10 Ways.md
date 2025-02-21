# How to remove Duplicate Data in SQL

#### **10 different ways to remove duplicate records in SQL. We look at 2 different scenario for duplicate records in a table and then come up with 10 SQL queries to remove these duplicate data from the database.**

*-> Data can be consider as duplicate if all column values are duplicated*

* *if all column values are duplicated*

**or**

* *if only some of the column values are duplicated.*

## Here you will find solution to remove duplicate data for both these scenarios. 

# Scenario 1: Data duplicated based on SOME of the columns 

### Requirement: Delete duplicate data from cars table. Duplicate record is identified based on the model and brand name.

**Table Structure**

```sql

drop table if exists cars;
create table if not exists cars
(
    id      int,
    model   varchar(50),
    brand   varchar(40),
    color   varchar(30),
    make    int
);
insert into cars values (1, 'Model S', 'Tesla', 'Blue', 2018);
insert into cars values (2, 'EQS', 'Mercedes-Benz', 'Black', 2022);
insert into cars values (3, 'iX', 'BMW', 'Red', 2022);
insert into cars values (4, 'Ioniq 5', 'Hyundai', 'White', 2021);
insert into cars values (5, 'Model S', 'Tesla', 'Silver', 2018);
insert into cars values (6, 'Ioniq 5', 'Hyundai', 'Green', 2021);

```

* **Solution Number : 1 ( Using Unique Idenifier) { when 2 Duplicate values are found }**

``` sql

-- First of all check similar data in consicutive rows

select * from cars
order by model, brand;

-- check in how many rows these 2 columns data are the same

select
model, brand, 
count(*) as cnt 
from cars 
group by model, brand;

-- check which group has >1 similar data and find MAX onece to delete it 

select
model, brand, 
max(id) as ids  
from cars 
group by model, brand
having count(*) >1;


-- now delete those IDs 

delete from cars 
	where id in ( 
select
max(id) as ids  
from cars 
group by model, brand
having count(*) >1)

```

* **Solution Number : 2 ( Using Self Join ) { when 2 Duplicate values are found }**

``` sql

-- These 3 steps will remains the same as previous solution, because after executing those queries,
--- we will get an idea that how many duplicates are there

-- First of all check similar data in consicutive rows
-- check in how many rows these 2 columns data are the same
-- check which group has >1 similar data and find MAX onece to delete it

-- SELF JOIN solution

delete from cars 
	where id in (
select 
	c1.id
from cars c 
join cars c1 on c.model = c1.model and 
                c.brand = c1.brand
where c.id < c1.id
	);

```

* **Solution Number : 3 ( Using Window Function) { Works with multiple duplicates }**

``` sql
delete from cars 
	where id in (
select X.id
	from (
select 
*,
row_number() over ( partition by model, brand ) as rnk 
from cars ) X
where X.rnk >1;
);
```

* **Solution 4 : Using MIN function. This deletes even multiple duplicate records.**

``` sql

-- first check that which Original IDs we want to keep as it is

select 
model, brand, min(id)
from cars 
group by 
model, brand

-- Now delete all IDs which are not in these record

delete from cars 
where id not in ( 
				select min(id)
				from cars 
				group by model, brand );

```

* **Solution Number 5 : Using Backup Table**

**When to use: currently we are practicing with very less amount of data, so in the real time, if we need to delete thousands of records after identifying duplicate records, it will take much time to delete it. So, that's why we can also use this technique, Because this will only consider Data without any duplicate values in the backup table which eventually makes lesser time to execute the query**

**When to not use: This methods reqires to delete Original table which has duplicate values, so if we are working on some important projects in the company and this data being used by applications, then we shouldn't directly delete this table, if we are working on just testing data in the company then only use this approach** 

``` sql

-- create a Backup table, and here we have used 1=2 filter because we just want to get table structure with empty data

create table  cars_bkp 
as
select * from cars where 1 = 2;

-- Then Insert Data Into Empty table

insert into cars_bkp
select * from cars 
where id in (select min(id)
			from cars 
			group by model, brand);

-- Now drop an Original Table

drop table cars;

-- Rename backup table

alter table cars_bkp rename to cars;

```

* **Solution Number 6 : Usng backup table without dropping the original table**

``` sql

-- here everything will remains the same as previous query, just need to Truncate the table ( will delete just data not entire table structure )
-- & Insers the data from backup table to Orginal Table

create table  cars_bkp 
as
select * from cars where 1 = 2;

insert into cars_bkp
select * from cars 
where id in (select min(id)
			from cars 
			group by model, brand);


truncate table cars; 

insert into cars 
select * from cars_bkp; 

```

















































```




