---
tags:
  - reading
---
Let’s explore window functions in SQL by building examples of increasing complexity. I’ll use a simple dataset and gradually introduce the nuances of window function syntax—PARTITION BY, ORDER BY, frame specifications (ROWS, RANGE, GROUPS), and exclusions. Each example will highlight a specific feature or variation. We'll assume a table called sales with the following structure and data:

sql

```sql
CREATE TABLE sales (
    sale_id INT,
    salesperson TEXT,
    sale_date DATE,
    amount NUMERIC
);

INSERT INTO sales VALUES
(1, 'Alice', '2025-01-01', 100),
(2, 'Alice', '2025-01-03', 150),
(3, 'Bob', '2025-01-02', 200),
(4, 'Bob', '2025-01-04', 300),
(5, 'Alice', '2025-01-05', 120),
(6, 'Bob', '2025-01-06', 250);
```

Example 1: Basic Window Function (Total Sum Over All Rows)

Let’s start with a simple window function to calculate the total sum of amount across all rows, without any grouping or ordering.

sql

```sql
SELECT 
    sale_id,
    salesperson,
    sale_date,
    amount,
    SUM(amount) OVER () AS total_sales
FROM sales;
```

Output:

```text
sale_id | salesperson | sale_date  | amount | total_sales
--------+-------------+------------+--------+-------------
1       | Alice       | 2025-01-01 | 100    | 1120
2       | Alice       | 2025-01-03 | 150    | 1120
3       | Bob         | 2025-01-02 | 200    | 1120
4       | Bob         | 2025-01-04 | 300    | 1120
5       | Alice       | 2025-01-05 | 120    | 1120
6       | Bob         | 2025-01-06 | 250    | 1120
```

Explanation:

- SUM(amount) OVER () computes the sum of amount over all rows in the table (1120).
    
- The empty OVER () clause means no partitioning or ordering—every row sees the full dataset.
    

---

Example 2: Partitioning with PARTITION BY

Now, let’s calculate the total sales per salesperson using PARTITION BY.

sql

```sql
SELECT 
    sale_id,
    salesperson,
    sale_date,
    amount,
    SUM(amount) OVER (PARTITION BY salesperson) AS total_by_salesperson
FROM sales;
```

Output:

```text
sale_id | salesperson | sale_date  | amount | total_by_salesperson
--------+-------------+------------+--------+---------------------
1       | Alice       | 2025-01-01 | 100    | 370
2       | Alice       | 2025-01-03 | 150    | 370
5       | Alice       | 2025-01-05 | 120    | 370
3       | Bob         | 2025-01-02 | 200    | 750
4       | Bob         | 2025-01-04 | 300    | 750
6       | Bob         | 2025-01-06 | 250    | 750
```

Explanation:

- PARTITION BY salesperson splits the rows into groups (Alice: 370, Bob: 750).
    
- Each row shows the sum of amount for its respective salesperson’s partition.
    

---

Example 3: Adding ORDER BY (Running Total)

Let’s compute a running total of sales per salesperson, ordered by sale_date.

sql

```sql
SELECT 
    sale_id,
    salesperson,
    sale_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY salesperson 
        ORDER BY sale_date
    ) AS running_total
FROM sales;
```

Output:

```text
sale_id | salesperson | sale_date  | amount | running_total
--------+-------------+------------+--------+---------------
1       | Alice       | 2025-01-01 | 100    | 100
2       | Alice       | 2025-01-03 | 150    | 250
5       | Alice       | 2025-01-05 | 120    | 370
3       | Bob         | 2025-01-02 | 200    | 200
4       | Bob         | 2025-01-04 | 300    | 500
6       | Bob         | 2025-01-06 | 250    | 750
```

Explanation:

- ORDER BY sale_date within the OVER clause makes SUM cumulative up to the current row within each partition.
    
- Default frame is RANGE UNBOUNDED PRECEDING to CURRENT ROW.
    

---

Example 4: Frame Specification with ROWS

Now, let’s use ROWS to compute the sum of the current and previous row within each partition.

sql

```sql
SELECT 
    sale_id,
    salesperson,
    sale_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY salesperson 
        ORDER BY sale_date
        ROWS BETWEEN 1 PRECEDING AND CURRENT ROW
    ) AS sum_prev_and_current
FROM sales;
```

Output:

```text
sale_id | salesperson | sale_date  | amount | sum_prev_and_current
--------+-------------+------------+--------+---------------------
1       | Alice       | 2025-01-01 | 100    | 100
2       | Alice       | 2025-01-03 | 150    | 250
5       | Alice       | 2025-01-05 | 120    | 270
3       | Bob         | 2025-01-02 | 200    | 200
4       | Bob         | 2025-01-04 | 300    | 500
6       | Bob         | 2025-01-06 | 250    | 550
```

Explanation:

- ROWS BETWEEN 1 PRECEDING AND CURRENT ROW limits the frame to the current row and the one before it.
    
- For the first row in each partition, only the current row is included (no prior row).
    

---

Example 5: RANGE Mode with Date Difference

Let’s use RANGE to sum sales within a 2-day range before the current sale_date.

sql

```sql
SELECT 
    sale_id,
    salesperson,
    sale_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY salesperson 
        ORDER BY sale_date
        RANGE BETWEEN INTERVAL '2 days' PRECEDING AND CURRENT ROW
    ) AS sum_within_2_days
FROM sales;
```

Output:

```text
sale_id | salesperson | sale_date  | amount | sum_within_2_days
--------+-------------+------------+--------+--------------------
1       | Alice       | 2025-01-01 | 100    | 100
2       | Alice       | 2025-01-03 | 150    | 250
5       | Alice       | 2025-01-05 | 120    | 270
3       | Bob         | 2025-01-02 | 200    | 200
4       | Bob         | 2025-01-04 | 300    | 500
6       | Bob         | 2025-01-06 | 250    | 550
```

Explanation:

- RANGE BETWEEN INTERVAL '2 days' PRECEDING AND CURRENT ROW includes rows where sale_date is within 2 days before the current row.
    
- E.g., for Alice on 2025-01-05, it includes 2025-01-03 (150) and 2025-01-05 (120).
    

---

Example 6: GROUPS Mode

Assume multiple sales can occur on the same date. Let’s modify the data slightly:

sql

```sql
INSERT INTO sales VALUES (7, 'Alice', '2025-01-03', 80);
```

Now, use GROUPS to sum over the current and previous group of dates:

sql

```sql
SELECT 
    sale_id,
    salesperson,
    sale_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY salesperson 
        ORDER BY sale_date
        GROUPS BETWEEN 1 PRECEDING AND CURRENT ROW
    ) AS sum_group_prev_and_current
FROM sales
ORDER BY salesperson, sale_date, sale_id;
```

Output:

```text
sale_id | salesperson | sale_date  | amount | sum_group_prev_and_current
--------+-------------+------------+--------+--------------------
1       | Alice       | 2025-01-01 | 100    | 100
2       | Alice       | 2025-01-03 | 150    | 330
7       | Alice       | 2025-01-03 | 80     | 330
5       | Alice       | 2025-01-05 | 120    | 350
3       | Bob         | 2025-01-02 | 200    | 200
4       | Bob         | 2025-01-04 | 300    | 500
6       | Bob         | 2025-01-06 | 250    | 550
```

Explanation:

- GROUPS BETWEEN 1 PRECEDING AND CURRENT ROW treats rows with the same sale_date as one group.
    
- For Alice on 2025-01-03, it includes 2025-01-01 (100) and 2025-01-03 (150 + 80).
    

---

Example 7: Frame Exclusion

Let’s exclude the current row from the running total:

sql

```sql
SELECT 
    sale_id,
    salesperson,
    sale_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY salesperson 
        ORDER BY sale_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
        EXCLUDE CURRENT ROW
    ) AS sum_excluding_current
FROM sales;
```

Output:

```text
sale_id | salesperson | sale_date  | amount | sum_excluding_current
--------+-------------+------------+--------+-----------------------
1       | Alice       | 2025-01-01 | 100    | 0
2       | Alice       | 2025-01-03 | 150    | 100
7       | Alice       | 2025-01-03 | 80     | 100
5       | Alice       | 2025-01-05 | 120    | 330
3       | Bob         | 2025-01-02 | 200    | 0
4       | Bob         | 2025-01-04 | 300    | 200
6       | Bob         | 2025-01-06 | 250    | 500
```

Explanation:

- EXCLUDE CURRENT ROW removes the current row’s amount from the sum.
    
- For Alice on 2025-01-05, it sums all prior rows (100 + 150 + 80 = 330).
    

---

Summary of Nuances

- No Frame (OVER ()): Entire partition, no order.
    
- PARTITION BY: Groups rows like GROUP BY.
    
- ORDER BY: Defines row order, enables running calculations.
    
- ROWS: Counts physical rows in the frame.
    
- RANGE: Uses value differences (e.g., dates or numbers).
    
- GROUPS: Treats equal-ordered rows as a single unit.
    
- Frame Exclusion: Fine-tunes which rows are included.
    

These examples should clarify how window functions evolve from simple aggregates to complex, row-specific calculations! Let me know if you’d like more variations.