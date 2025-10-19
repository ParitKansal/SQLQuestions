## 🔢 Problem: Find the Start and End Number of Continuous Ranges

### **Table Schema**

#### `Logs`

```sql
+-------------+------+
| Column Name | Type |
+-------------+------+
| log_id      | int  |
+-------------+------+
log_id is the primary key for this table.
Each row represents a unique log entry ID.
```

### **Problem Description**

Write an SQL query to find the **start and end numbers of continuous ranges** in the `Logs` table.

* Continuous means consecutive `log_id`s — for example, `1, 2, 3, 4` form one continuous range.
* A break (e.g., jump from 4 to 7) indicates a new range starts.

Return the result table ordered by the `start_id`.

### **Return Format**

| start_id | end_id |
| -------- | ------ |

### **Example Input**

#### `Logs`

| log_id |
| ------ |
| 1      |
| 2      |
| 3      |
| 7      |
| 8      |
| 10     |

### **Example Output**

| start_id | end_id |
| -------- | ------ |
| 1        | 3      |
| 7        | 8      |
| 10       | 10     |

---
