# LeetCode Notes - SQL

## 601 Human Traffic of Staduim

Created on: 04/01/2021

Modified on: 04/05/2021

---

### Difficulty

Hard

## Instructions

Table: `Stadium`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| visit_date    | date    |
| people        | int     |
+---------------+---------+
visit_date is the primary key for this table.
Each row of this table contains the visit date and visit id to the stadium with 
the number of people during the visit.
No two rows will have the same visit_date, and as the id increases, the dates 
increase as well.
```

Write an SQL query to display the records with three or more rows with 
**consecutive** id's, and the number of people is greater than or equal to 
100 for each.

Return the result table ordered by `visit_date` in **ascending order**.

The query result format is in the following example.

```
Stadium table:
+------+------------+-----------+
| id   | visit_date | people    |
+------+------------+-----------+
| 1    | 2017-01-01 | 10        |
| 2    | 2017-01-02 | 109       |
| 3    | 2017-01-03 | 150       |
| 4    | 2017-01-04 | 99        |
| 5    | 2017-01-05 | 145       |
| 6    | 2017-01-06 | 1455      |
| 7    | 2017-01-07 | 199       |
| 8    | 2017-01-09 | 188       |
+------+------------+-----------+

Result table:
+------+------------+-----------+
| id   | visit_date | people    |
+------+------------+-----------+
| 5    | 2017-01-05 | 145       |
| 6    | 2017-01-06 | 1455      |
| 7    | 2017-01-07 | 199       |
| 8    | 2017-01-09 | 188       |
+------+------------+-----------+
The four rows with ids 5, 6, 7, and 8 have consecutive ids and each of them 
has >= 100 people attended. Note that row 8 was included even though the 
visit_date was not the next day after row 7.
The rows with ids 2 and 3 are not included because we need at least three 
consecutive ids.
```

## Solution

```sql
SELECT
    DISTINCT s1.*
FROM Stadium AS s1
JOIN Stadium AS s2
JOIN Stadium AS s3
ON (
    (s1.id = s2.id - 1 AND s1.id = s3.id - 2) OR
    (s1.id = s2.id + 1 AND s1.id = s3.id - 1) OR
    (s1.id = s2.id + 1 AND s1.id = s3.id + 2)
)
WHERE
    s1.people >= 100 AND
    s2.people >= 100 AND
    s3.people >= 100
ORDER BY s1.visit_date ASC;
```

## Explanation

The trick here is to link three tables and find at least three consecutive days. 
To achieve this, we need to join three tables either by `visit_date` or by `id`.

## Notes

- `JOIN`
