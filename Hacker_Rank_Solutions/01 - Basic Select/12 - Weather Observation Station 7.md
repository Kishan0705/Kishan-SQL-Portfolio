## Q - Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.

```sql
SELECT 
DISTINCT CITY
FROM STATION 
WHERE RIGHT(CITY,1) IN ('a','e','i','o','u')
