## Problem: Drop Type 1 Orders for Customers With Type 0 Orders

**Schema**

```sql
Table: Orders
+-+-+-+
| Column Name | Type        |
+-+-+-+
| order_id    | INT         |
| customer_id | INT         |
| order_type  | INT         |
+-+-+-+
order_id is the primary key.
```

Each order has a `customer_id` and an `order_type`, which is either **0** or **1**.



### Problem Statement

Write an SQL query to **report all the orders** subject to this rule:

* If a customer has **at least one** order of **type 0**, then **do not report any** of that customer’s orders of **type 1**.
* Otherwise (i.e. if a customer has **no type-0 orders**), report **all** of that customer’s orders (of type 0 or 1).

Return the result in **any order**.



### Example

**Input:**

Orders table:

| order_id | customer_id | order_type |
| -- | -- | - |
| 1        | 1           | 0          |
| 2        | 1           | 0          |
| 11       | 2           | 0          |
| 12       | 2           | 1          |
| 21       | 3           | 1          |
| 22       | 3           | 0          |
| 31       | 4           | 1          |
| 32       | 4           | 1          |

**Output:**

| order_id | customer_id | order_type |
| -- | -- | - |
| 1        | 1           | 0          |
| 2        | 1           | 0          |
| 11       | 2           | 0          |
| 22       | 3           | 0          |
| 31       | 4           | 1          |
| 32       | 4           | 1          |

**Explanation:**

* Customer 1 has only type 0 orders → return them.
* Customer 2 has both type 0 and type 1 orders → but since they have type 0, we drop their type 1 (so only order_id=11 remains).
* Customer 3 also has both types → drop the type 1 (so only order_id=22).
* Customer 4 has only type 1 orders (no type 0) → return them (both order_id 31 and 32).

---

**SOLUTION:**

```sql
WITH table1 AS(
  SELECT *
  FROM Orders
  WHERE order_type = 0
),
table2 AS(
  SELECT *
  FROM Orders
  WHERE order_type = 1
),
table3 AS(
  SELECT table2.order_id, table2.customer_id, table2.order_type
  FROM table2
  LEFT JOIN table1
  ON table1.customer_id = table2.customer_id
  WHERE table1.customer_id IS NULL
)
SELECT *
FROM table1
UNION
SELECT *
FROM table3
;
```
