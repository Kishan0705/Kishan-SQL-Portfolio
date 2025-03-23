# How to remove Duplicate Data in SQL

---

 ## **➟ [ 1 ] : ROW_NUMBER() with CTE**  
 
   Best for **large datasets** & **optimized performance**  

```sql
WITH cte AS (  
    SELECT  
        id,  
        ROW_NUMBER() OVER (PARTITION BY name, age, grade ORDER BY id) AS rnk  
    FROM Students  
)  
DELETE FROM Students  
WHERE id IN (  
    SELECT id FROM cte WHERE rnk > 1  
);
```
1️⃣ We use **ROW_NUMBER()** to assign a unique number to each record within the same group (`name, age, grade`).  

2️⃣ The **first occurrence** of each group gets `ROW_NUMBER() = 1`, and duplicates get `ROW_NUMBER() > 1`.  

3️⃣ A **CTE (Common Table Expression)** stores this result temporarily.  

4️⃣ Finally, we **delete all records** where `ROW_NUMBER() > 1`, keeping only the first occurrence.  

-> This method is **fast, scalable, and works well for large datasets!**  

---

 ## **➟ [ 2 ] : Self-Join Approach**  

```sql
DELETE s1  
FROM Students s1  
JOIN Students s2  
ON s1.name = s2.name  
AND s1.age = s2.age  
AND s1.grade = s2.grade  
WHERE s1.id > s2.id;
```

---

1️⃣ **Joins the table with itself** to find duplicate records.  
2️⃣ **Matches records with the same** `name, age, grade`.  
3️⃣ **Deletes records where `id > MIN(id)`**, keeping only the first occurrence.  

 **Universal method** → Works in **all SQL databases** without advanced functions!  

---

 ## **➟ [ 3 ] : DELETE with EXISTS**  

```sql
DELETE FROM Employees e1  
WHERE EXISTS (  
    SELECT 1  
    FROM Employees e2  
    WHERE e1.name = e2.name  
    AND e1.salary = e2.salary  
    AND e1.id > e2.id  
);
```

---

1️⃣ **Checks for duplicates** using a correlated subquery.  
2️⃣ **If a duplicate exists**, the outer query deletes the extra record (`id > MIN(id)`).  
3️⃣ **More optimized than a JOIN** in some databases because it stops searching once a match is found.  

 **Efficient for large datasets** and works across **most SQL databases**!   

---

 ## **➟ [ 4 ] : Backup & TRUNCATE Method**  

```sql
CREATE TABLE backup_employees AS  
SELECT MIN(id) AS id, name, salary  
FROM Employees  
GROUP BY name, salary;  

TRUNCATE TABLE Employees;  

INSERT INTO Employees  
SELECT * FROM backup_employees;  

DROP TABLE backup_employees;
```

---

1️⃣ **Creates a backup table** with only **unique records** (`MIN(id)`).  
2️⃣ **Truncates the original table** (fastest way to remove all data).  
3️⃣ **Restores unique records** from the backup table.  
4️⃣ **Drops the temporary backup table** after restoration.  

 **Best for:**  
 
✅ **Data safety** – Ensures a full backup before deletion.  
✅ **Large datasets** – `TRUNCATE` is faster than `DELETE`.  
✅ **Universal** – Works in all SQL databases.  

---

 ## **➟ [ 5 ] : NOT IN with MIN(id) Method**

```sql
DELETE FROM Orders  
WHERE id NOT IN (  
    SELECT MIN(id)  
    FROM Orders  
    GROUP BY item, order_date  
);
```

---

1️⃣ **Finds the smallest `id`** (`MIN(id)`) for each duplicate group (`item, order_date`).  
2️⃣ **Deletes all other duplicate records** while keeping the first occurrence.  
3️⃣ **Simple and effective** for smaller datasets.  

 **Best for:**  
✅ **Clear and easy to understand** SQL syntax.  
✅ **Works across all SQL databases (universal approach).**  
✅ **Good for small to medium-sized datasets.**  

⚠ **Caution:** `NOT IN` can be slower on large datasets. Consider using `JOIN` or `EXISTS` for better performance.  

---



# Scenario 2: Data duplicated based on ALL of the columns

If **all row values are completely duplicate** (including `id`), then **normal methods won't work** because `id` alone **can't be used to identify duplicates**.  

### ✅ **Universal & Optimized Solution for Fully Duplicate Rows**  
1️⃣ **Use `GROUP BY` with `COUNT(*)` to identify duplicates**  
2️⃣ **Use `ROW_NUMBER()` (if supported) or `SELF JOIN` to delete extra rows**  

---

### **🔹 ROW_NUMBER() Approach (Best for Large Datasets)**
```sql
WITH cte AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY name, age, grade ORDER BY id) AS rnk  
    FROM Students
)  
DELETE FROM Students  
WHERE id IN (SELECT id FROM cte WHERE rnk > 1);
```
✅ **Best Performance**  
✅ **Works on large datasets**  
❌ **Not in MySQL <8.0 & SQLite**  

---

### **🔹 Universal Solution (Works in All RDBMS)**
👉 **Self-Join to Delete Duplicate Rows Without Unique ID**
```sql
DELETE s1  
FROM Students s1  
JOIN Students s2  
ON s1.name = s2.name  
AND s1.age = s2.age  
AND s1.grade = s2.grade  
WHERE s1.id > s2.id;
```
✅ **Works in all databases**  
✅ **Does not require ROW_NUMBER()**  
❌ **Slightly less efficient than ROW_NUMBER()**  

---

### **🔹 Alternative: Create a New Table with DISTINCT Rows**  
If deleting duplicates is tricky, the safest method is **creating a clean table**:  
```sql
CREATE TABLE new_students AS  
SELECT DISTINCT * FROM Students;  

DROP TABLE Students;  

ALTER TABLE new_students RENAME TO Students;
```
✅ **Guaranteed to remove duplicates**  
✅ **Works everywhere**  
❌ **Requires table recreation (not always feasible)**  

---

### **💡 Best Choice?**
- **For large datasets → Use `ROW_NUMBER()` (if supported)**  
- **For any RDBMS → Use Self-Join**  
- **For guaranteed removal → Use `DISTINCT` and recreate the table**  





















































```




