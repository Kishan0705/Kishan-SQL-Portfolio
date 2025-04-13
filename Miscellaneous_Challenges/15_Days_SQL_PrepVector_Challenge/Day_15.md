

### **Day 15: Duplicate Transaction**  
**Difficulty**: Hard

---

### **Problem Statement**

You're given a `transactions` table containing transaction records. Your task is to **identify repeated payments** defined as:

- Same **merchant**
- Same **credit card**
- Same **amount**
- Made within **10 minutes** (600 seconds) of each other

> **Important Assumption**:  
> The **first transaction** in a duplicate pair should **not be counted** as a repeated payment.  
> For example, if two identical transactions happen within 10 minutes, only **1** should be counted as a repeated payment.

---

### **Given Tables**

```sql
CREATE TABLE transactions (
  id INT PRIMARY KEY,
  credit_card VARCHAR(20),
  merchant VARCHAR(50),
  amount DECIMAL(10, 2),
  transaction_time DATETIME
);

INSERT INTO transactions (id, credit_card, merchant, amount, transaction_time)
VALUES
(1, '1234-5678-9876', 'Amazon', 50.00, '2025-01-23 10:15:00'),
(2, '1234-5678-9876', 'Amazon', 50.00, '2025-01-23 10:20:00'),
(3, '5678-1234-8765', 'Walmart', 30.00, '2025-01-23 11:00:00'),
(4, '1234-5678-9876', 'Amazon', 50.00, '2025-01-23 10:30:00'),
(5, '5678-1234-8765', 'Walmart', 30.00, '2025-01-23 11:05:00'),
(6, '8765-4321-1234', 'BestBuy', 100.00, '2025-01-23 12:00:00'),
(7, '1234-5678-9876', 'Amazon', 50.00, '2025-01-23 12:10:00');
```

---



### **SQL Query**

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

### **Sample Output**

| repeated_payment_count |
|------------------------|
| 3                      |

---
