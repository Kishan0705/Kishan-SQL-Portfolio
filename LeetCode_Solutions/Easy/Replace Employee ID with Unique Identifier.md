### Replace Employee ID with Unique Identifier

**Problem Statement:**  
Write a solution to show the unique ID of each user.  
If a user does not have a unique ID, return `null`.

Return the result table in any order.

---

**Example Input:**

Employees Table  
| id  | name     |
|-----|----------|
| 1   | Alice    |
| 7   | Bob      |
| 11  | Meir     |
| 90  | Winston  |
| 3   | Jonathan |

EmployeeUNI Table  
| id  | unique_id |
|-----|-----------|
| 3   | 1         |
| 11  | 2         |
| 90  | 3         |

---

**SQL Query:**
```sql
SELECT 
  u.unique_id, 
  e.name     
FROM Employees e 
LEFT JOIN EmployeeUNI u
  ON e.id = u.id;
```
