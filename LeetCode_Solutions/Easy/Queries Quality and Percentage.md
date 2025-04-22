### Queries Quality and Poor Query Percentage

**Problem Statement:**  
We define **query quality** as the average of the ratio between query `rating` and its `position`.  
We also define **poor query percentage** as the percentage of all queries with `rating < 3`.

Write a solution to find each `query_name`, its corresponding `quality`, and `poor_query_percentage`, rounded to 2 decimal places.

**Example Input:**

Queries Table  
| query_name | result           | position | rating |
|------------|------------------|----------|--------|
| Dog        | Golden Retriever | 1        | 5      |
| Dog        | German Shepherd  | 2        | 5      |
| Dog        | Mule             | 200      | 1      |
| Cat        | Shirazi          | 5        | 2      |
| Cat        | Siamese          | 3        | 3      |
| Cat        | Sphynx           | 7        | 4      |

**SQL Query:**
```sql
SELECT 
  query_name,
  ROUND(AVG(rating * 1.0 / position), 2) AS quality,
  ROUND(SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS poor_query_percentage
FROM Queries
GROUP BY query_name;
```

**Expected Output:**

| query_name | quality | poor_query_percentage |
|------------|---------|------------------------|
| Dog        | 2.52    | 33.33                  |
| Cat        | 0.83    | 33.33                  |
