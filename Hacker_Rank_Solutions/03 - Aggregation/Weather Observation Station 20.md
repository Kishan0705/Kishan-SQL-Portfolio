# [ 17 ]

- A median is defined as a number separating the higher half of a data set from the lower half. Query the median of the Northern Latitudes (LAT_N) from STATION and round your answer to 4 decimal places.

```sql

WITH Ranked_Locations AS (
    SELECT 
        LAT_N,
        RANK() OVER (ORDER BY LAT_N) AS rank_position
    FROM STATION
),
Median_Position AS ( 
    SELECT 
        FLOOR((MAX(rank_position) + 1) / 2) AS median_rank
    FROM Ranked_Locations
)
SELECT 
    ROUND(rl.LAT_N, 4) AS Median_Value 
FROM Ranked_Locations rl
JOIN Median_Position mp
ON rl.rank_position = mp.median_rank;

```
