## Problem: Capital Gain/Loss

**Schema**

```sql
Table: Stocks  
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| stock_name     | varchar |
| operation      | enum    |  — values: “Buy” or “Sell”  
| operation_day  | int     |
| price          | int     |
+----------------+---------+

The primary key is (stock_name, operation_day).
```

### Problem Statement

Calculate the **capital gain or loss** for each stock.

* Each row in `Stocks` represents either a `Buy` or `Sell` operation for a stock, with a price and day.
* It is guaranteed that for each `Sell` operation, there is a corresponding `Buy` operation earlier, and vice versa eventually.
* You should sum up all gains and losses (i.e., `Sell` adds value, `Buy` subtracts value) grouped by `stock_name`.

Return a table:

| stock_name | capital_gain_loss |

in **any order**.

### Example

Suppose `Stocks` is:

| stock_name | operation | operation_day | price |
| ---------- | --------- | ------------- | ----- |
| AAPL       | Buy       | 1             | 100   |
| AAPL       | Sell      | 3             | 150   |
| GOOG       | Buy       | 2             | 200   |
| GOOG       | Sell      | 5             | 250   |
| AAPL       | Buy       | 4             | 120   |
| AAPL       | Sell      | 6             | 130   |

Then:

* For AAPL: gain = (150 − 100) + (130 − 120) = 60
* For GOOG: gain = (250 − 200) = 50

Output:

| stock_name | capital_gain_loss |
| ---------- | ----------------- |
| AAPL       | 60                |
| GOOG       | 50                |

---

```sql
# Write your MySQL query statement below
SELECT 
    stock_name,
    SUM(
        CASE
            WHEN operation = 'Buy'
                THEN -price
            ELSE
                price 
        END
    )AS capital_gain_loss
FROM Stocks
GROUP BY stock_name
;
```
