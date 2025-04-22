### Average Selling Price

**Problem Statement:**  
Find the average selling price for each product, based on the price at the time of sale.  
If a product has no sales, its average selling price is assumed to be 0.  
Round the result to 2 decimal places.

**Example Input:**

Prices Table  
| product_id | start_date | end_date   | price |
|------------|------------|------------|-------|
| 1          | 2019-02-17 | 2019-02-28 | 5     |
| 1          | 2019-03-01 | 2019-03-22 | 20    |
| 2          | 2019-02-01 | 2019-02-20 | 15    |
| 2          | 2019-02-21 | 2019-03-31 | 30    |

UnitsSold Table  
| product_id | purchase_date | units |
|------------|----------------|-------|
| 1          | 2019-02-25     | 100   |
| 1          | 2019-03-01     | 15    |
| 2          | 2019-02-10     | 200   |
| 2          | 2019-03-22     | 30    |

**SQL Query:**
```sql
SELECT 
  p.product_id,
  ROUND(COALESCE(SUM(u.units * p.price) / SUM(u.units), 0), 2) AS average_price
FROM prices p
LEFT JOIN UnitsSold u 
  ON p.product_id = u.product_id 
  AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY p.product_id;
```
