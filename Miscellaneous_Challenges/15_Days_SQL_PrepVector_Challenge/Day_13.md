

### **Day 13: Clickthrough Rate by Age**  
**Difficulty**: Hard

---

### **Problem Statement**

You’re given two tables:  
- `users(id, name, birthdate)`  
- `search_events(search_id, query, has_clicked, user_id, search_time)`  

Your task is to find the **top 3 age groups** (bucketed by decade: 0–9, 10–19, 20–29, ..., 90–99) with the **highest Clickthrough Rate (CTR)** in **2021**.

> Note:  
> A user’s age group depends on their age **on the exact date of the search**.  
> For example, if someone is 29 on January 1, 2021, they fall in the [20–29] group.  
> But if the same user searches again on February 1, 2021 (after their birthday), and turns 30, they now fall into the [30–39] group.


---
### Data

```sql
CREATE TABLE users (
id INTEGER PRIMARY KEY,
name VARCHAR(100),
birthdate DATETIME
);

INSERT INTO users (id, name, birthdate) VALUES
(1, 'Alice', '1995-05-15'),
(2, 'Bob', '1985-03-20'),
(3, 'Charlie', '2005-07-10'),
(4, 'David', '1955-11-30'),
(5, 'Eve', '2015-09-25'),
(6, 'Frank', '1935-02-14'),
(7, 'Grace', '1975-12-01');

CREATE TABLE search_events (
search_id INTEGER PRIMARY KEY,
query VARCHAR(255),
has_clicked BOOLEAN,
user_id INTEGER,
search_time DATETIME,
FOREIGN KEY (user_id) REFERENCES users(id)
);

INSERT INTO search_events (search_id, query, has_clicked, user_id, search_time) VALUES

(1, 'travel', TRUE, 1, '2021-03-15 10:00:00'),
(2, 'books', FALSE, 1, '2021-03-15 11:00:00'),
(3, 'cars', TRUE, 2, '2021-05-20 14:30:00'),
(4, 'tech', TRUE, 2, '2021-05-20 15:00:00'),
(5, 'games', FALSE, 3, '2021-07-10 16:45:00'),
(6, 'music', FALSE, 3, '2021-07-10 17:00:00'),
(7, 'retirement', TRUE, 4, '2021-09-05 09:15:00'),
(8, 'health', FALSE, 4, '2021-09-05 10:00:00'),
(9, 'toys', FALSE, 5, '2021-11-25 13:20:00'),
(10, 'genealogy', TRUE, 6, '2021-12-01 11:30:00'),
(11, 'history', TRUE, 6, '2021-12-01 12:00:00'),
(12, 'finance', TRUE, 7, '2021-02-15 08:45:00'),
(13, 'investing', FALSE, 7, '2021-02-15 09:00:00');
```


### **SQL Query**

```sql
WITH events_2021 AS (
  SELECT 
    se.user_id,
    se.has_clicked,
    se.search_time,
    u.birthdate,
    (CAST(strftime('%Y', se.search_time) AS INTEGER) - CAST(strftime('%Y', u.birthdate) AS INTEGER)) 
    - (strftime('%m-%d', se.search_time) < strftime('%m-%d', u.birthdate)) AS age_at_search
  FROM search_events se
  JOIN users u ON se.user_id = u.id
  WHERE strftime('%Y', se.search_time) = '2021'
),
age_bucketed AS (
  SELECT 
    user_id,
    has_clicked,
    search_time,
    age_at_search,
    CAST(age_at_search / 10 AS INTEGER) AS age_group
  FROM events_2021
),
ctr_by_age_group AS (
  SELECT 
    age_group,
    ROUND(100.0 * SUM(CASE WHEN has_clicked THEN 1 ELSE 0 END) / NULLIF(COUNT(*), 0), 2) AS ctr
  FROM age_bucketed
  GROUP BY age_group
)
SELECT 
  CAST(age_group * 10 AS TEXT) || '-' || CAST(age_group * 10 + 9 AS TEXT) AS age_groups,
  ctr
FROM ctr_by_age_group
ORDER BY ctr DESC, age_groups DESC
LIMIT 3;



```


---
