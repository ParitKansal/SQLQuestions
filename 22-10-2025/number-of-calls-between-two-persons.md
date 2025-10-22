Perfect 👍 — you’re referring to the **LeetCode SQL problem #1699 — “Number of Calls Between Two Persons.”**
Here’s the **complete formatted problem statement**, **example input**, and **ready-to-run MySQL setup + query template** for you to practice and understand fully 👇

---

## ☎️ Problem: Number of Calls Between Two Persons

### **Table Schema**

#### `Calls`

```sql
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| from_id     | int     |
| to_id       | int     |
| duration    | int     |
+-------------+---------+
There is no primary key (multiple calls possible).
Each row represents a call between from_id and to_id with its duration in minutes.
```

---

### 🧠 **Problem Description**

Write an SQL query to find the **total number of calls** and the **total duration** between each unique pair of people.

* Calls are **bidirectional**:
  a call from A → B is the same as a call from B → A.
* Each pair should appear **only once** in the result.

Return the result table **ordered by person1_id, then person2_id**.

---

### **Return Format**

| person1 | person2 | call_count | total_duration |
| ------- | ------- | ---------- | -------------- |

---

### **Example Input**

#### `Calls`

| from_id | to_id | duration |
| ------- | ----- | -------- |
| 1       | 2     | 59       |
| 2       | 1     | 11       |
| 1       | 3     | 20       |
| 3       | 4     | 100      |
| 3       | 4     | 200      |
| 3       | 4     | 200      |
| 4       | 3     | 499      |

---

### **Example Output**

| person1 | person2 | call_count | total_duration |
| ------- | ------- | ---------- | -------------- |
| 1       | 2       | 2          | 70             |
| 1       | 3       | 1          | 20             |
| 3       | 4       | 4          | 999            |

---

```sql
WITH table1 AS (
  SELECT 
    duration,
    LEAST(from_id, to_id) AS person1,
    GREATEST(from_id, to_id) AS person2
  FROM Calls
)
SELECT
  person1 AS person1_id, 
  person2 AS person2_id, 
  COUNT(duration) AS call_count, 
  SUM(duration) AS total_duration
FROM table1
GROUP BY person1, person2
ORDER BY person1_id, person2_id;
```



