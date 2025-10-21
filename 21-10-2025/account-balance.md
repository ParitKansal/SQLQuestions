## 🏦 Problem: Account Balance

### **Table Schema**

#### `Transactions`

```sql
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| account_id    | int     |
| transaction_id| int     |
| type          | ENUM('Deposit', 'Withdraw') |
| amount        | int     |
| day           | date    |
+---------------+---------+
(transaction_id) is the primary key.
Each row represents a single transaction.
```

### 🧠 **Problem Description**

You are given a table `Transactions` containing records of deposits and withdrawals made by various accounts.

Write a query to calculate the **current balance** of each account after all its transactions have been processed.

Return the result table **sorted by account_id**.

### **Return Format**

| account_id | balance |
| ---------- | ------- |

### **Example Input**

#### `Transactions`

| account_id | transaction_id | type     | amount | day        |
| ---------- | -------------- | -------- | ------ | ---------- |
| 1          | 101            | Deposit  | 2000   | 2023-01-01 |
| 1          | 102            | Withdraw | 1000   | 2023-01-02 |
| 2          | 103            | Deposit  | 3000   | 2023-01-01 |
| 2          | 104            | Withdraw | 500    | 2023-01-02 |
| 3          | 105            | Deposit  | 1000   | 2023-01-03 |

### **Example Output**

| account_id | balance |
| ---------- | ------- |
| 1          | 1000    |
| 2          | 2500    |
| 3          | 1000    |

---

**SOLUTION**:
```sql
SELECT account_id,
SUM(CASE
  WHEN type = 'Deposit' THEN +amount
  ELSE - amount
END) AS balance
FROM  Transactions
GROUP BY account_id
ORDER BY balance
```
