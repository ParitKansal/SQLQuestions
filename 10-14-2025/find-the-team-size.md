## Problem: Find the Team Size

**Schema**
```sql
Table: Employee
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| employee_id | int     |
| team_id     | int     |
+-------------+---------+
employee_id is the primary key for this table.
Each row of this table indicates the team that each employee belongs to.
```

### **Problem Statement**
- Write an SQL query to find the **team size of each employee**.
- Return the result table in **any order**.
- Each employee should have a column showing how many members (including themselves) are in their team.

### **Example**
**Input**
Employee table:
| employee_id | team_id |
| ----------- | ------- |
| 1           | 8       |
| 2           | 8       |
| 3           | 8       |
| 4           | 7       |
| 5           | 9       |
| 6           | 9       |

**Output**
| employee_id | team_size |
| ----------- | --------- |
| 1           | 3         |
| 2           | 3         |
| 3           | 3         |
| 4           | 1         |
| 5           | 2         |
| 6           | 2         |

### **Explanation**
* Team **8** has 3 members (employees 1, 2, 3)
* Team **7** has 1 member (employee 4)
* Team **9** has 2 members (employees 5, 6)
So each employee is paired with the size of their team.
---
**SOLUTION:**
```sql
WITH table1 AS (
  SELECT
    team_id,
    COUNT(employee_id) AS team_size
  FROM Employee
  GROUP BY team_id
)
SELECT
  employee_id, team_size
FROM Employee
LEFT JOIN table1
ON Employee.team_id = table1.team_id
;
```

```sql
SELECT
  employee_id,
  COUNT(*) OVER (PARTITION BY team_id) AS team_size
FROM Employee
;
```
