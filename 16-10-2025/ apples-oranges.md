## Problem: Apples & Oranges

**Schema**

```sql
Table: Sales  
+-------------+--------+-----------+
| Column Name | Type   |
+-------------+--------+-----------+
| sale_date   | date   |
| fruit       | enum   |  — values: 'apples' or 'oranges'  
| sold_num    | int    |
+-------------+--------+-----------+

(sale_date, fruit) is the primary key.
```

**Problem Statement**
Each row in `Sales` gives the number of fruits sold (either apples or oranges) on a particular `sale_date`.

You need to write an SQL query to report, for **each day**, the **difference** between the number of apples and oranges sold (i.e. apples minus oranges).

Return the result table with columns:

| sale_date | diff |

* `diff` = (# apples sold) − (# oranges sold)
* Order the output by `sale_date` ascending.

**Example**

Input:

| sale_date  | fruit   | sold_num |
| ---------- | ------- | -------- |
| 2020-05-01 | apples  | 10       |
| 2020-05-01 | oranges | 8        |
| 2020-05-02 | apples  | 15       |
| 2020-05-02 | oranges | 15       |
| 2020-05-03 | apples  | 20       |
| 2020-05-03 | oranges | 0        |
| 2020-05-04 | apples  | 15       |
| 2020-05-04 | oranges | 16       |

Output:

| sale_date  | diff |
| ---------- | ---- |
| 2020-05-01 | 2    |
| 2020-05-02 | 0    |
| 2020-05-03 | 20   |
| 2020-05-04 | −1   |

Explanation:

* On 2020-05-01: 10 apples, 8 oranges → diff = 10 − 8 = 2
* On 2020-05-02: 15 apples, 15 oranges → diff = 0
* On 2020-05-03: 20 apples, 0 oranges → diff = 20
* On 2020-05-04: 15 apples, 16 oranges → diff = −1

---

**SOLUTION**:
```sql

WITH table1 AS (
  SELECT *
  FROM Sales
  WHERE fruit = 'oranges'
),
table2 AS (
  SELECT *
  FROM Sales
  WHERE fruit = 'apples'
),
table3 AS (
  SELECT 
    table2.sale_date, 
    table2.sold_num AS apples, 
    table1.sold_num AS oranges
  FROM table2
  LEFT JOIN table1
    ON table2.sale_date = table1.sale_date
  
  UNION
  
  SELECT 
    table1.sale_date, 
    table2.sold_num AS apples, 
    table1.sold_num AS oranges
  FROM table2
  RIGHT JOIN table1
    ON table2.sale_date = table1.sale_date
)
SELECT
  sale_date,
  CASE
    WHEN oranges IS NOT NULL AND apples IS NOT NULL THEN apples - oranges
    WHEN oranges IS NULL AND apples IS NOT NULL THEN apples - 0
    WHEN oranges IS NOT NULL AND apples IS NULL THEN 0 - oranges
  END AS diff
FROM table3
ORDER BY sale_date;
```




