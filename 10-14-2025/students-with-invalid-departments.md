## Problem: Students With Invalid Departments

**Schema**

```sql
Table: Departments
+------------+-------------+
| Column Name| Type        |
+------------+-------------+
| id         | INT         |
| name       | VARCHAR     |
+------------+-------------+
id is the primary key.

Table: Students
+--------------+-------------+
| Column Name  | Type        |
+--------------+-------------+
| id           | INT         |
| name         | VARCHAR     |
| department_id| INT         |
+--------------+-------------+
id is the primary key.
```

**Problem Statement**
Write an SQL query to find the **id** and **name** of all students who are enrolled in departments that **no longer exist** (i.e., their `department_id` is not present in the `Departments` table).

Return the result table in **any order**.

**Example**

Departments table:

| id | name                    |
| -- | ----------------------- |
| 1  | Electrical Engineering  |
| 7  | Computer Engineering    |
| 13 | Business Administration |

Students table:

| id | name     | department_id |
| -- | -------- | ------------- |
| 23 | Alice    | 1             |
| 1  | Bob      | 7             |
| 5  | Jennifer | 13            |
| 2  | John     | 14            |
| 4  | Jasmine  | 77            |
| 3  | Steve    | 74            |
| 6  | Luis     | 1             |
| 8  | Jonathan | 7             |
| 7  | Daiana   | 33            |
| 11 | Madelynn | 1             |

Expected output:

| id | name    |
| -- | ------- |
| 2  | John    |
| 4  | Jasmine |
| 3  | Steve   |
| 7  | Daiana  |

**Explanation**
John, Jasmine, Steve, and Daiana are enrolled in departments with IDs 14, 77, 74, and 33 — none of which exist in the `Departments` table.

---

**SOLUTION**:

```sql
WITH table1 AS (
  SELECT s.id AS student_id, s.name AS student_name, s.department_id AS department_id, d.name AS department_name 
  FROM Students AS s
  LEFT JOIN
  Departments AS d
  ON s.department_id = d.id
),
table2 AS (
  SELECT * FROM
  table1
  WHERE department_name IS NULL
)
SELECT student_id, student_name
FROM table2
GROUP BY student_id 
;
```

```sql
WITH table1 AS (
  SELECT s.id AS student_id, s.department_id AS department_id, d.name AS department_name 
  FROM Students AS s
  LEFT JOIN Departments AS d
  ON s.department_id = d.id
  WHERE department_name IS NULL
),
table2 AS (
  SELECT DISTINCT(student_id) AS student_id
  FROM table1
)
SELECT table2.student_id AS id, Students.name AS name 
FROM table2
LEFT JOIN Students
ON table2.student_id = Students.id
;
```

```sql
SELECT DISTINCT Students.id, Students.name
FROM Students
LEFT JOIN Departments
ON Students.department_id = Departments.id
WHERE Departments.name IS NULL
;
```







