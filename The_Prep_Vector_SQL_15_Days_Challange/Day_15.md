
---

# Day 15 of 15: The PrepVector SQL Challenge  
**Approach Used:** `SELF JOIN`, `ABS()`, `strftime('%s')`, `JOIN Conditions`, `COUNT()`

---

## 📌 Problem Statement:

Using the **transactions** table, identify any payments made at the **same merchant**, using the **same credit card**, for the **same amount**, within **10 minutes of each other**.

---

## 🧠 Thought Process:

We need to detect suspicious duplicate transactions that happen too close to each other.  

So here's how I tackled it step by step:

---

### ➤ **Step 1: Self Join to Compare Transactions**  
To compare a transaction against all others, I joined the **transactions** table with itself, but with a twist.  
I used `t.id < t1.id` to make sure each pair is checked only once and we don’t match a transaction with itself.

---

### ➤ **Step 2: Match Key Fields**  
We only want to consider transactions that match on:  
• Same Merchant  
• Same Credit Card  
• Same Amount  

This ensures we’re comparing exact duplicates.

---

### ➤ **Step 3: Check the Time Gap**  
The critical part was checking if the difference between the two transaction times was ≤ 10 minutes.  
So I used `strftime('%s', ...)` to convert timestamps to seconds and applied `ABS(...) <= 600`.

---

### ➤ **Step 4: Count Only Repeated Payments**  
Important! We only want to count the second transaction, not the first one.  
That’s why the condition `t.id < t1.id` matters — it ensures we treat `t1` as the repeat.  

Finally, I just counted the repeated payments.

---

## ✅ SQL Query Solution:
```sql
SELECT
    COUNT(*) AS repeated_payment_count
FROM (
    SELECT
        t.id AS tx1,
        t1.id AS tx2
    FROM transactions t
    JOIN transactions t1
        ON t.merchant = t1.merchant
        AND t.credit_card = t1.credit_card
        AND t.amount = t1.amount
        AND t.id < t1.id
        AND ABS(strftime('%s', t.transaction_time) - strftime('%s', t1.transaction_time)) <= 600
);
```

---

## 🔑 Key Takeaway:

• When comparing rows within the same table, **self joins** can be powerful, especially for identifying patterns across transactions.  
• Convert timestamps to seconds using `strftime('%s', ...)` to perform accurate time difference calculations in SQLite.

---

