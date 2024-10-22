## Q - Query the list of CITY names from STATION that either do not start with vowels or do not end with vowels. Your result cannot contain duplicates.

```sql
SELECT 
DISTINCT CITY
FROM STATION 
WHERE LEFT(CITY,1) NOT IN ('a','e','i','o','u') OR 
      RIGHT(CITY,1) NOT IN ('a','e','i','o','u')
