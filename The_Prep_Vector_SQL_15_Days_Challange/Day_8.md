
---

# Day 8 of 15: The PrepVector SQL Challenge  
**Approach Used:** `SELF JOIN`, `JOIN`, `DATE()`

---

## 📌 Problem Statement:

Write a query to return pairs of projects where the end date of one project matches the start date of another project.

---

## 🧠 Thought Process:

• Used a self-join to match projects where end date of one project equals the start date of another.  

• Selected project titles and the matching date to identify linked projects.

---

## ✅ SQL Query Solution:
```sql
SELECT
    p.title AS project_title_end,
    p1.title AS project_title_start,
    DATE(p.end_date) AS date
FROM projects p
JOIN projects p1
    ON p.end_date = p1.start_date;
```

---

## 🔑 Key Takeaway:

Self-joins are an important topic in many SQL interviews, helping to compare rows within the same table. They are useful for finding patterns, tracking sequences, and linking related data efficiently.

---

