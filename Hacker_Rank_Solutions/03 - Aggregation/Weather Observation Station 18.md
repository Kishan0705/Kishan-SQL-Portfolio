# [ 15 ] 

- Consider p1( a,b )  and p2( c,d ) to be two points on a 2D plane.

 - a happens to equal the minimum value in Northern Latitude (LAT_N in STATION).
 - b happens to equal the minimum value in Western Longitude (LONG_W in STATION).
 - c happens to equal the maximum value in Northern Latitude (LAT_N in STATION).
 - d happens to equal the maximum value in Western Longitude (LONG_W in STATION).

Query the Manhattan Distance between points p1 and p2 and round it to a scale of 4 decimal places.

```sql
SELECT
ROUND((MAX(LAT_N) + MAX(LONG_W)) - 
(MIN(LAT_N)+ MIN(LONG_W)),4)
FROM STATION
```
