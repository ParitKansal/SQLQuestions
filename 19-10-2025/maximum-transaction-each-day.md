## 💳 Problem: Maximum Transaction Each Day

### **Table Schema**

#### `Transactions`

```sql
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| transaction_id | int  |
| customer_id    | int  |
| transaction_date | date |
| amount         | int  |
+-------------+---------+
(transaction_id) is the primary key for this table.
Each row contains the amount of a single transaction by a customer on a specific date.
```


## 🧠 Problem Description

Write an SQL query to find the **maximum transaction amount per day**.

Return the result table in **any order**.

### **Return Format**

| transaction_date | amount |
| ---------------- | ------ |

### **Example Input**

#### `Transactions`

| transaction_id | customer_id | transaction_date | amount |
| -------------- | ----------- | ---------------- | ------ |
| 1              | 101         | 2024-01-01       | 150    |
| 2              | 102         | 2024-01-01       | 200    |
| 3              | 101         | 2024-01-02       | 50     |
| 4              | 103         | 2024-01-02       | 300    |
| 5              | 102         | 2024-01-03       | 250    |

### **Example Output**

| transaction_date | amount |
| ---------------- | ------ |
| 2024-01-01       | 200    |
| 2024-01-02       | 300    |
| 2024-01-03       | 250    |


---


**SOLUTION**:

```sql
SELECT transaction_date, MAX(amount) AS amount
FROM Transactions
GROUP BY transaction_date
```



