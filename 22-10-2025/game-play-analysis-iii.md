### Schema

Table: `Activity`

| Column Name                                                  | Type |
| ------------------------------------------------------------ | ---- |
| player_id                                                    | int  |
| device_id                                                    | int  |
| event_date                                                   | date |
| games_played                                                 | int  |
| *(player_id, event_date) is the primary key) ([Leetcode][1]) |      |

### Problem Description

Write an SQL query to report for **each player** and **each event_date**, how many games they have played **so far** up to that date.

* That is, for a given `player_id` and `event_date`, compute the cumulative sum of `games_played` for that player up to and including that date. ([Medium][2])
* Return columns: `player_id`, `event_date`, `games_played_so_far`.
* Order by `player_id`, then `event_date`. ([Walkccc][3])

### Example Input

`Activity`

| player_id | device_id | event_date | games_played |
| --------- | --------- | ---------- | ------------ |
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-05-02 | 6            |
| 1         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |

### Example Output

| player_id | event_date | games_played_so_far |
| --------- | ---------- | ------------------- |
| 1         | 2016-03-01 | 5                   |
| 1         | 2016-05-02 | 11                  |
| 1         | 2017-06-25 | 12                  |
| 3         | 2016-03-02 | 0                   |
| 3         | 2018-07-03 | 5                   |

---
**SOLUTION**:
```sql
SELECT
  player_id, 
  event_date,
  SUM(games_played) OVER(
    PARTITION BY player_id
    ORDER BY event_date
    ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
  ) AS games_played_so_far
FROM Activity
ORDER BY player_id, event_date
;
```
