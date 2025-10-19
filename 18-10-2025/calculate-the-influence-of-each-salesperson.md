## 🧮 Problem: Calculate the Influence of Each Salesperson

### **Table Schema**

#### `Salesperson`

```sql
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| salesperson_id| int     |
| name          | varchar |
+---------------+---------+
salesperson_id is the primary key.
Each row of this table indicates the name and ID of a salesperson.
```

#### `Customer`

```sql
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| customer_id  | int     |
| salesperson_id | int   |
| name         | varchar |
+--------------+---------+
customer_id is the primary key.
Each customer is assigned to exactly one salesperson.
```

#### `Orders`

```sql
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| customer_id   | int     |
| product_id    | int     |
| order_date    | date    |
| quantity      | int     |
+---------------+---------+
order_id is the primary key.
Each row represents a single product purchase by a customer.
```

#### `Product`

```sql
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| name          | varchar |
| price         | int     |
+---------------+---------+
product_id is the primary key.
Each row contains the name and price of a product.
```

---

### 🧠 **Problem Description**

A salesperson’s **influence** is defined as the **total sales amount** made by all their assigned customers.

The total sales amount for a customer =
sum of (`price × quantity`) for all their orders.

You must **calculate the total influence** for each salesperson in the `Salesperson` table.

Return the result table containing:
| salesperson_id | name | total_influence |

If a salesperson has **no orders** (i.e., none of their customers placed any), the `total_influence` should be **0**.

Return the results in **any order**.

---

### 💡 **Example Input**

#### `Salesperson`

| salesperson_id | name    |
| -------------- | ------- |
| 1              | Alice   |
| 2              | Bob     |
| 3              | Charlie |

#### `Customer`

| customer_id | salesperson_id | name |
| ----------- | -------------- | ---- |
| 1           | 1              | Sam  |
| 2           | 1              | Joe  |
| 3           | 2              | Max  |

#### `Orders`

| order_id | customer_id | product_id | order_date | quantity |
| -------- | ----------- | ---------- | ---------- | -------- |
| 1        | 1           | 1          | 2024-01-01 | 3        |
| 2        | 2           | 2          | 2024-02-01 | 1        |
| 3        | 3           | 1          | 2024-02-03 | 2        |

#### `Product`
 
| product_id | name | price |
| ---------- | ---- | ----- |
| 1          | Pen  | 10    |
| 2          | Book | 50    |

---

### 📊 **Example Output**

| salesperson_id | name    | total_influence |
| -------------- | ------- | --------------- |
| 1              | Alice   | 80              |
| 2              | Bob     | 20              |
| 3              | Charlie | 0               |

---

**SOLUTION**:
```sql
WITH table1 AS (
  SELECT  
      order_id, 
      customer_id, 
      quantity * price AS amount 
  FROM Orders
  LEFT JOIN Product 
      ON Orders.product_id = Product.product_id
),
table2 AS(
  SELECT 
      customer_id, 
      SUM(amount) AS Total
  FROM table1
  GROUP BY customer_id
),
table3 AS(
  SELECT Customer.customer_id, salesperson_id, Total
  FROM Customer
  LEFT JOIN table2
  ON Customer.customer_id = table2.customer_id
),
table4 AS(
SELECT  Salesperson.salesperson_id, Salesperson.name, 
  case
    WHEN table3.Total IS null
      then 0
    else
      table3.Total
  END AS total_influence
  -- table3.Total AS total_influence
  FROM Salesperson
  LEFT JOIN table3
  on Salesperson.salesperson_id = table3.salesperson_id
)
select salesperson_id, name, SUM(total_influence) AS total_influence
FROM table4
GROUP BY salesperson_id, name
```






