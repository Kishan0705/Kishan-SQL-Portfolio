## Problem Statement:  
### Unfinished Parts - Tesla SQL Interview Question  

Tesla is investigating production bottlenecks and needs your help to extract the relevant data. Write a query to determine which parts have begun the assembly process but are not yet finished.

### Assumptions:  
- The `parts_assembly` table contains all parts currently in production, each at varying stages of the assembly process.  
- An unfinished part is one that lacks a `finish_date`.  
- This question is straightforward, so let's approach it with simplicity in both thinking and solution.  

### SQL Solution:
```sql
SELECT 
    part, 
    assembly_step 
FROM parts_assembly 
WHERE finish_date IS NULL;
