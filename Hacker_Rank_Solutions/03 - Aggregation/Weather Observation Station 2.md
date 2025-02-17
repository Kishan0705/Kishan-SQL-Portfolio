# [ 1 ] 

- The sum of all values in LAT_N rounded to a scale of 2 decimal places.
- The sum of all values in LONG_W rounded to a scale of 2 decimal places.

```sql
select 
ROUND(sum(LAT_N),2),
ROUND(sum(LONG_W),2)
FROM STATION
```
