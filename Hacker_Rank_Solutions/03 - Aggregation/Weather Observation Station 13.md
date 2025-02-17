# [ 2 ]

- Query the sum of Northern Latitudes (LAT_N) from STATION having values greater than 38.7880 and less than 137.2315. Truncate your answer to 4 decimal places.

```sql
SELECT ROUND(SUM(LAT_N), 4) AS Lat_N  
FROM STATION  
WHERE LAT_N > 38.7880 AND LAT_N < 137.2345;
```
