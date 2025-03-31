## Problem Statement: Cities With Completed Trades
### Robinhood SQL Interview Question

Assume you're given the tables containing completed trade orders and user details in a Robinhood trading system.

Write a query to retrieve the top three cities that have the highest number of completed trade orders listed in descending order. Output the city name and the corresponding number of completed trade orders.

### SQL Query:

```sql
SELECT 
    u.city,
    SUM( CASE WHEN t.status = 'Completed' THEN 1 END ) AS total_orders
FROM trades t 
JOIN users u 
ON t.user_id = u.user_id
GROUP BY u.city
ORDER BY total_orders DESC
LIMIT 3;
