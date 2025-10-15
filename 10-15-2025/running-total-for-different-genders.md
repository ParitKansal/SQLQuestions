## Problem: Running Total for Different Genders

**Schema**

```sql
Table: Scores  
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| player_name   | VARCHAR |
| gender        | VARCHAR |
| day           | DATE    |
| score_points  | INT     |
+---------------+---------+
Primary key: (gender, day)  
```

Each row indicates that a player (with a given `gender`) scored `score_points` on `day`.

### Problem Statement

Write an SQL query to find the **running total score** (cumulative sum) for each gender, day by day.

* For each gender, on each day in ascending order, compute the sum of `score_points` from the **earliest day up to the current day** for that gender.
* Return columns `(gender, day, total)`.
* Order the final result by `gender` ascending, and `day` ascending.

### Example

Suppose `Scores` is:

| player_name | gender | day        | score_points |
| ----------- | ------ | ---------- | ------------ |
| Priyanka    | F      | 2019-12-30 | 17           |
| Priya       | F      | 2019-12-31 | 23           |
| Aron        | F      | 2020-01-01 | 17           |
| Alice       | F      | 2020-01-07 | 23           |
| Jose        | M      | 2019-12-18 | 2            |
| Khali       | M      | 2019-12-25 | 11           |
| Slaman      | M      | 2019-12-30 | 13           |
| Joe         | M      | 2019-12-31 | 3            |
| Bajrang     | M      | 2020-01-07 | 7            |

Output should be:

| gender | day        | total |
| ------ | ---------- | ----- |
| F      | 2019-12-30 | 17    |
| F      | 2019-12-31 | 40    |
| F      | 2020-01-01 | 57    |
| F      | 2020-01-07 | 80    |
| M      | 2019-12-18 | 2     |
| M      | 2019-12-25 | 13    |
| M      | 2019-12-30 | 26    |
| M      | 2019-12-31 | 29    |
| M      | 2020-01-07 | 36    |

Explanation:

* For gender = F, by 2019-12-31, total = 17 + 23 = 40; by 2020-01-07, total = 17 + 23 + 17 + 23 = 80, etc.
* Similarly for gender = M.

---

**SOLUTION:**
```sql
WITH table1 AS(
    SELECT 
        gender, 
        day, 
        SUM(score_points) AS total
    FROM DailySales
    Group BY day, gender
)
SELECT 
    gender, 
    day,
    SUM(total) OVER(
        PARTITION BY gender 
        ORDER BY day 
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS total
FROM table1
ORDER BY gender, day
```



