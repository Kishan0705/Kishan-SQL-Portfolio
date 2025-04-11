## Day 8: Sequential Project Pairs
**Difficulty:** Medium

### Problem
Write a query to return pairs of projects where the end date of one project matches the start date of another project.

---

### Given Data
```sql
CREATE TABLE projects (
  id INTEGER PRIMARY KEY,
  title VARCHAR(100),
  start_date DATETIME,
  end_date DATETIME,
  budget FLOAT
);

INSERT INTO projects (id, title, start_date, end_date, budget) VALUES
(1, 'Website Redesign', '2024-01-01', '2024-02-15', 50000),
(2, 'Mobile App Phase 1', '2024-02-15', '2024-04-01', 75000),
(3, 'Database Migration', '2024-04-01', '2024-05-15', 60000),
(4, 'Cloud Integration', '2024-03-01', '2024-04-15', 45000),
(5, 'Security Audit', '2024-05-15', '2024-06-30', 30000);
```

---

### Solution
```sql
SELECT
    p.title AS project_title_end,
    p1.title AS project_title_start,
    DATE(p.end_date) AS date
FROM projects p  
JOIN projects p1  
ON p.end_date = p1.start_date;
```

This query joins the `projects` table with itself to find pairs where the `end_date` of one project is exactly equal to the `start_date` of another project. The result shows the titles of both projects and the date that connects them.

