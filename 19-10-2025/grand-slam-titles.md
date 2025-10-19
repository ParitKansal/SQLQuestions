## 🎾 Problem: Grand Slam Titles

### **Table Schema**

#### `Players`

```sql
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| player_id   | int     |
| player_name | varchar |
+-------------+---------+
player_id is the primary key for this table.
Each row contains the ID and name of a tennis player.
```

#### `Championships`

```sql
+-------------------+---------+
| Column Name       | Type    |
+-------------------+---------+
| year              | int     |
| Wimbledon         | int     |
| Fr_open           | int     |
| US_open           | int     |
| Au_open           | int     |
+-------------------+---------+
year is the primary key for this table.
Each column other than year contains the player_id of the champion of that tournament in that year.
```

## 🧠 Problem Description

You are given a table `Players` with player names and a table `Championships` that records the winners of all four major tennis tournaments — **Wimbledon**, **French Open (Fr_open)**, **US Open (US_open)**, and **Australian Open (Au_open)** — for each year.

Write an SQL query to report the number of **grand slam titles** each player has won.
A player’s total count includes wins across all years and all tournaments.

Return the result table ordered by:

1. The number of titles in **descending** order, and
2. The player’s name in **ascending** order if there’s a tie.

### **Return Format**

| player_id | player_name | grand_slams_count |
| --------- | ----------- | ----------------- |

### **Example Input**

#### `Players`

| player_id | player_name |
| --------- | ----------- |
| 1         | Novak       |
| 2         | Rafael      |
| 3         | Roger       |
| 4         | Andy        |

#### `Championships`

| year | Wimbledon | Fr_open | US_open | Au_open |
| ---- | --------- | ------- | ------- | ------- |
| 2018 | 1         | 2       | 3       | 4       |
| 2019 | 1         | 2       | 1       | 2       |
| 2020 | 2         | 2       | 3       | 4       |

---

### **Example Output**

| player_id | player_name | grand_slams_count |
| --------- | ----------- | ----------------- |
| 2         | Rafael      | 4                 |
| 1         | Novak       | 3                 |
| 3         | Roger       | 2                 |
| 4         | Andy        | 2                 |

---
**SOLUTION**:
```sql
WITH table1 AS (
  SELECT year, Wimbledon AS winner
  FROM Championships
  UNION ALL
  SELECT year, Fr_open AS winner
  FROM Championships
  UNION ALL
  SELECT year, US_open AS winner
  FROM Championships
  UNION ALL
  SELECT year, Au_open AS winner
  FROM Championships
),
table2 AS (
  SELECT winner, COUNT(year) AS slams_count
  FROM table1
  GROUP BY winner
)
SELECT  
  p.player_id, 
  p.player_name, 
  COALESCE(t.slams_count, 0) AS grand_slams_count
FROM Players p
LEFT JOIN table2 t
  ON p.player_id = t.winner
ORDER BY grand_slams_count DESC, player_name ASC;
```





