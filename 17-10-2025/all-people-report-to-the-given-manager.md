## 🧾 Problem: All People Report to the Given Manager

**Schema**

```sql
Table: Employees
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| employee_id | int      |
| name        | varchar  |
| manager_id  | int      |
+-------------+----------+
employee_id is the primary key for this table.
This table contains the ID of each employee and the ID of their direct manager.
```

### 📄 Problem Statement

Write an SQL query to find all **employees** who **directly or indirectly** report to a **given manager** (i.e., a manager with `manager_id = 1`).

* A direct report means that the employee’s `manager_id` equals the given manager’s `employee_id`.
* An indirect report means that the employee reports to another employee who is in turn (directly or indirectly) managed by the given manager.

Return the result table in **any order**.

**Result Format:**

| employee_id | name |
| ----------- | ---- |

### 🧩 Example

**Input:**

| employee_id | name  | manager_id |
| ----------- | ----- | ---------- |
| 1           | John  | NULL       |
| 2           | Dan   | 1          |
| 3           | James | 2          |
| 4           | Amy   | 3          |
| 5           | Anne  | 4          |
| 6           | Ron   | 1          |

**Given:** `manager_id = 1`

**Output:**

| employee_id | name  |
| ----------- | ----- |
| 2           | Dan   |
| 3           | James |
| 4           | Amy   |
| 5           | Anne  |
| 6           | Ron   |

---
**SOLUTION**:
```sql
WITH RECURSIVE 
hierarchy AS (
    SELECT 
        e.employee_id, 
        e.name, 
        e.manager_id
    FROM Employees e
    WHERE e.manager_id IN (1)

    UNION

    SELECT 
        e.employee_id, 
        e.name, 
        h.manager_id
    FROM Employees e
    INNER JOIN hierarchy h
        ON e.manager_id = h.employee_id
)
SELECT DISTINCT 
    employee_id, 
    name,
    manager_id
FROM hierarchy
ORDER BY employee_id;

```  


```sql
WITH RECURSIVE 
table1 AS (
    SELECT employee_id
    FROM Employees
    WHERE manager_id IS NULL
),
hierarchy AS (
    SELECT 
        e.employee_id, 
        e.name, 
        e.manager_id
    FROM Employees e
    WHERE e.manager_id IN (SELECT employee_id FROM table1)

    UNION

    SELECT 
        e.employee_id, 
        e.name, 
        h.manager_id
    FROM Employees e
    INNER JOIN hierarchy h
        ON e.manager_id = h.employee_id
)
SELECT DISTINCT 
    employee_id, 
    name,
    manager_id
FROM hierarchy
ORDER BY employee_id;
```

```sql
WITH RECURSIVE 
table1 AS (
    SELECT employee_id
    FROM Employees
    WHERE manager_id IS NULL
),
hierarchy AS (
    SELECT 
        e.employee_id, 
        e.name, 
        e.manager_id
    FROM Employees e
    WHERE e.manager_id IN (SELECT employee_id FROM table1)

    UNION

    SELECT 
        e.employee_id, 
        e.name, 
        h.manager_id
    FROM Employees e
    INNER JOIN hierarchy h
        ON e.manager_id = h.employee_id
)
SELECT employee_id, name, GROUP_CONCAT( manager_id) AS managers
FROM hierarchy
GROUP BY employee_id, name
;
```
