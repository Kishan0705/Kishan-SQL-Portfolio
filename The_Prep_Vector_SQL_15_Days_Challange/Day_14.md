
---

# Day 14 of 15: The PrepVector SQL Challenge  
**Approach Used:** `CTE`, `SUM()`, `JOIN`, `LAG()`, `Window Functions`, `ROUND()`

---

## 📌 Problem Statement:

Given a table of **transactions** and **products**, write a query to get the **month-over-month change in revenue for the year 2019**.  
Make sure to **round month-over-month to 2 decimal places**.

---

## 🧠 Thought Process:

When I read the problem, the first thing that hit me was:  

**Okay, I need to calculate how much revenue changed from one month to the next in 2019 — that’s Month-over-Month (MoM) analysis!**  

But for that, I needed two things:  

➊ **Monthly revenue**  
➋ **The previous month’s revenue**, so I can calculate the % difference  

So here's how I broke it down step-by-step 👇  

---

### ➟ **Step 1: Extracting the Month and Calculating Monthly Revenue**  
I used `strftime('%m', t.created_at)` to extract the month from the transaction date, and cast it as an integer for sorting.  

Then I calculated revenue using `SUM(price * quantity)` by joining the **products** and **transactions** tables.  

Grouped it all by month to get monthly revenue totals ✅  

---

### ➟ **Step 2: Calculating the Month-over-Month Change**  
This was the core logic.  

I used the **LAG()** window function to access the revenue of the previous month, then simply applied the MoM formula:  

\[
\text{MoM Change %} = \frac{\text{Current Month Revenue - Previous Month Revenue}}{\text{Previous Month Revenue}} \times 100
\]

---

### ➟ **Final Output:**  
Now each row gives me:  
• The month number  
• The % change in revenue from the previous month  

Exactly what the problem asked for 💯  

---

## ✅ SQL Query Solution:
```sql
WITH monthly_sales AS (
    SELECT
        CAST(strftime('%m', t.created_at) AS INTEGER) AS month,
        SUM(p.price * t.quantity) AS total_sales
    FROM products p
    JOIN transactions t
        ON p.id = t.product_id
    WHERE strftime('%Y', t.created_at) = '2019'
    GROUP BY month
)
SELECT
    month,
    ROUND(
        (total_sales - LAG(total_sales) OVER (ORDER BY month)) * 100.0 /
        LAG(total_sales) OVER (ORDER BY month),
        2
    ) AS month_over_month
FROM monthly_sales;
```

---

## 🔑 Key Takeaway:

• Want to compare **trends over time**? → Use **Window functions like LAG()**.  

---

