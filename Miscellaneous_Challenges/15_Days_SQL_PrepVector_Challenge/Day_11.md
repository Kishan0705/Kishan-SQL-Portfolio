

## Day 11: Third Unique Song Play Date  
**Difficulty**: Hard

### Problem  
Given a table of `song_plays` and a table of `users`, write a query to extract the **earliest date** each user played their **third unique song** and order the results by the date it was played.

---

### Given Data  
```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username VARCHAR(50)
);

INSERT INTO users (id, username) VALUES
(1, 'john_doe'),
(2, 'jane_smith'),
(3, 'bob_wilson');

CREATE TABLE song_plays (
    id INTEGER PRIMARY KEY,
    played_at DATETIME,
    user_id INTEGER,
    song_id INTEGER
);

INSERT INTO song_plays (id, played_at, user_id, song_id) VALUES
(1, '2024-01-01 10:00:00', 1, 101),
(2, '2024-01-01 14:00:00', 1, 101),
(3, '2024-01-02 09:00:00', 1, 102),
(4, '2024-01-03 16:00:00', 1, 103),
(5, '2024-01-04 11:00:00', 1, 104),
(6, '2024-01-01 09:00:00', 2, 201),
(7, '2024-01-01 15:00:00', 2, 202),
(8, '2024-01-02 10:00:00', 2, 203),
(9, '2024-01-02 14:00:00', 2, 203),
(10, '2024-01-01 12:00:00', 3, 301),
(11, '2024-01-02 13:00:00', 3, 302);
```

---

### Solution  
```sql
WITH song_ranking AS (
    SELECT 
        user_id,
        song_id,
        first_played,
        ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY first_played) AS rnk
    FROM (
        SELECT 
            user_id,
            song_id,
            MIN(played_at) AS first_played
        FROM song_plays
        GROUP BY user_id, song_id
    ) AS user_first_played_song
)
SELECT 
    u.username,
    s.song_id,
    s.first_played
FROM song_ranking s
JOIN users u ON s.user_id = u.id
WHERE rnk = 3
ORDER BY first_played;
```

---

