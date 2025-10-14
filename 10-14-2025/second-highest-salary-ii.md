## Problem: Second Highest Salary II

**Schema**

```sql
Table: Employee
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | INT  |
| salary      | INT  |
+-------------+------+

id is the primary key.
```

### Problem Statement

Write an SQL query to obtain the second highest **distinct** salary from the `Employee` table.
If there is no second highest salary (for example, when all employees have the same salary or there is only one employee), the query should return `NULL`.

Return the result in a one-column table:

| SecondHighestSalary |
| ------------------- |

### Example

**Input**

| id | salary |
| -- | ------ |
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

**Output**

| SecondHighestSalary |
| ------------------- |
| 200                 |

### Important Details / Constraints

* Salaries may be duplicated across employees — we care about **distinct** values.
* If there is no valid “second highest” value (fewer than two distinct salaries), return `NULL`.
* Many SQL dialects support window functions (like `DENSE_RANK`, `ROW_NUMBER`, etc.), subqueries, `LIMIT`/`OFFSET`, etc., which are often used in solutions.
---

```sql
WITH table1 AS(
    SELECT
      DISTINCT salary
    FROM Employee 
),
table2 AS(
    SELECT
      *,
      ROW_NUMBER() OVER(ORDER BY salary DESC) AS rank_
    FROM table1
),
table3 AS(
    SELECT
      *
    FROM table2
    WHERE rank_ = 2   
),
table4 AS(
    SELECT
      2 as rank_ 
)
SELECT
  salary AS SecondHighestSalary
FROM table4
LEFT JOIN table3
ON table4.rank_ = table3.rank_
```
