## Problem Statement:

### **Pharmacy Analytics (Part 2)**  
**CVS Health SQL Interview Question**  

CVS Health is analyzing its pharmacy sales data and evaluating how well different products are selling in the market. Each drug is exclusively manufactured by a single manufacturer.

Write a query to identify the manufacturers associated with the drugs that resulted in losses for CVS Health and calculate the total amount of losses incurred.

**Output:**  
- Manufacturer's name
- Number of drugs associated with losses
- Total losses in absolute value
- Display results sorted in descending order, with the highest losses at the top.

### **SQL Query:**
```sql
SELECT 
    manufacturer,
    COUNT(drug) AS drug_count,
    SUM(cogs - total_sales) AS total_loss
FROM pharmacy_sales 
WHERE cogs > total_sales
GROUP BY manufacturer
ORDER BY total_loss DESC;
