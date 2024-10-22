## Q - Query all attributes of every Japanese city in the CITY table. The COUNTRYCODE for Japan is JPN.

```sql
SELECT * 
FROM CITY 
WHERE COUNTRYCODE  = 'JPN'
