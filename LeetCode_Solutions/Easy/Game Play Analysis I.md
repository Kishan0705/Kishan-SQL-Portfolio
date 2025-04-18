# SQL Problem: Game Play Analysis I

## Problem Statement

You are given a table: `Activity`.

### Table: Activity

| Column Name | Type    |
|-------------|---------|
| player_id   | int     |
| device_id   | int     |
| event_date  | date    |
| games_played| int     |

Each row is a record of the games played by a player on a specific date using a specific device.

## Objective

Write a solution to find the first login date for each player.

## Input

### Activity table:

| player_id | device_id | event_date | games_played |
|-----------|-----------|------------|--------------|
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-05-02 | 6            |
| 2         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |

## SQL Query

```sql
WITH cte AS (
  SELECT 
    player_id, 
    event_date,
    ROW_NUMBER() OVER (PARTITION BY player_id ORDER BY event_date) AS rnk 
  FROM Activity 
)
SELECT 
  player_id, 
  event_date AS first_login 
FROM cte 
WHERE rnk = 1;
