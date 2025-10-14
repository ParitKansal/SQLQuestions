Here’s the **formatted version** of the **“Class Performance”** SQL problem from LeetCode (Problem #2989) — without the solution, so you can attempt it yourself:

---

## Problem: Class Performance

**Schema**

```sql
Table: Scores  
+---------------+---------------+  
| Column Name   | Type          |  
+---------------+---------------+  
| student_id    | INT           |  
| student_name  | VARCHAR       |  
| assignment1   | INT           |  
| assignment2   | INT           |  
| assignment3   | INT           |  
+---------------+---------------+  
student_id is the primary key.  
```

**Problem Statement**
Write an SQL query to compute the difference between the **highest total score** and the **lowest total score** among all students.
The total score for each student is the sum of their `assignment1 + assignment2 + assignment3`.

Return a single-row result with column `difference_in_score`.

**Expected Output Structure**

| difference_in_score |
| ------------------- |

**Example**

Input (Scores table):

| student_id | student_name | assignment1 | assignment2 | assignment3 |
| ---------- | ------------ | ----------- | ----------- | ----------- |
| 309        | Owen         | 88          | 47          | 87          |
| 321        | Claire       | 98          | 95          | 37          |
| 338        | Julian       | 100         | 64          | 43          |
| 423        | Peyton       | 60          | 44          | 47          |
| 896        | David        | 32          | 37          | 50          |
| 235        | Camila       | 31          | 53          | 69          |

* Owen’s total = 88 + 47 + 87 = 222
* Claire’s total = 98 + 95 + 37 = 230
* Julian’s total = 100 + 64 + 43 = 207
* Peyton’s total = 60 + 44 + 47 = 151
* David’s total = 32 + 37 + 50 = 119
* Camila’s total = 31 + 53 + 69 = 153

The highest total is 230 (Claire), the lowest is 119 (David). The difference is `230 – 119 = 111`.

So the output:

| difference_in_score |
| ------------------- |
| 111                 |

---
**SOLUTION:**
```
WITH table1 AS(
    SELECT assignment1+assignment2+assignment3 AS SCORE
    FROM Scores  
)
SELECT MAX(SCORE) - MIN(SCORE) AS difference_in_score
FROM table1
;
```
