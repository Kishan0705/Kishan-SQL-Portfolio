## Problem Statement:  

### Duplicate Job Listings  
**LinkedIn SQL Interview Question**  

Assume you're given a table containing job postings from various companies on the LinkedIn platform. Write a query to retrieve the count of companies that have posted duplicate job listings.  

**Definition:**  
Duplicate job listings are defined as two job listings within the same company that share identical titles and descriptions.

---

### SQL Solution  

```sql
WITH job_count_cte AS (
  SELECT 
    company_id, 
    title, 
    description, 
    COUNT(job_id) AS job_count
  FROM job_listings
  GROUP BY company_id, title, description
)

SELECT COUNT(DISTINCT company_id) AS duplicate_companies
FROM job_count_cte
WHERE job_count > 1;
