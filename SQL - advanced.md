---
tags:
  - reading
---


| source                                           |     |
| ------------------------------------------------ | --- |
| https://www.crunchydata.com/developers/tutorials |     |
| https://www.windowfunctions.com/                 |     |
| https://use-the-index-luke.com/                  |     |
| https://pgexercises.com/                         |     |


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
    


-----

Below is a progression of examples in **PostgreSQL** illustrating the various nuances of window functions. Each step builds upon the previous concepts, going from basic PARTITION BY usage to advanced frame clauses (ROWS, RANGE, GROUPS) and exclusions.

---

## 1. Setup a Simple Table

We'll use a simple `sales` table:

```sql
CREATE TABLE sales (
    id          SERIAL PRIMARY KEY,
    region      TEXT,
    product     TEXT,
    sales_date  DATE,
    sales_amt   NUMERIC
);
```

Insert some sample data:

```sql
INSERT INTO sales (region, product, sales_date, sales_amt) VALUES
('East', 'Widget', '2023-01-01', 100),
('East', 'Widget', '2023-01-02', 200),
('East', 'Gadget', '2023-01-02', 150),
('West', 'Widget', '2023-01-01', 300),
('West', 'Gadget','2023-01-02', 400),
('East', 'Gadget','2023-01-03', 100),
('West', 'Widget', '2023-01-03', 50),
('East', 'Widget', '2023-01-04', 250);
```

Let's assume the final contents of `sales` are:

|id|region|product|sales_date|sales_amt|
|---|---|---|---|---|
|1|East|Widget|2023-01-01|100|
|2|East|Widget|2023-01-02|200|
|3|East|Gadget|2023-01-02|150|
|4|West|Widget|2023-01-01|300|
|5|West|Gadget|2023-01-02|400|
|6|East|Gadget|2023-01-03|100|
|7|West|Widget|2023-01-03|50|
|8|East|Widget|2023-01-04|250|

---

## 2. Basic Window Functions (No PARTITION, No ORDER)

**Goal**: Show a window function without any `PARTITION BY` or `ORDER BY`.

**Query**:

```sql
SELECT
    id,
    region,
    product,
    sales_date,
    sales_amt,
    SUM(sales_amt) OVER () AS total_sales
FROM sales;
```

- `SUM(sales_amt) OVER ()` calculates the sum of `sales_amt` **across all rows** (no partition).
- The same `total_sales` value is repeated on every row.

**Result (excerpt)**:

|id|region|product|sales_date|sales_amt|total_sales|
|---|---|---|---|---|---|
|1|East|Widget|2023-01-01|100|1550|
|2|East|Widget|2023-01-02|200|1550|
|3|East|Gadget|2023-01-02|150|1550|
|4|West|Widget|2023-01-01|300|1550|
|5|West|Gadget|2023-01-02|400|1550|
|6|East|Gadget|2023-01-03|100|1550|
|7|West|Widget|2023-01-03|50|1550|
|8|East|Widget|2023-01-04|250|1550|

(Here, 1550 is the sum of `100 + 200 + 150 + 300 + 400 + 100 + 50 + 250`.)

---

## 3. PARTITION BY

**Goal**: Show how to calculate an aggregate function for each partition (group) independently.

**Query**:

```sql
SELECT
    region,
    product,
    sales_date,
    sales_amt,
    SUM(sales_amt) OVER (PARTITION BY region) AS sum_by_region
FROM sales
ORDER BY region, sales_date, product;
```

Explanation:

- `PARTITION BY region` breaks the result into two partitions: East, West.
- Each row will display `sum_by_region` that sums only within its partition (region).
- No `ORDER BY` clause inside `OVER()` means no specific ordering for the window calculation. (The `ORDER BY region, sales_date, product` is just for final output presentation at the query level, not in the window function.)

**Result (excerpt)**:

|region|product|sales_date|sales_amt|sum_by_region|
|---|---|---|---|---|
|East|Widget|2023-01-01|100|800|
|East|Widget|2023-01-02|200|800|
|East|Gadget|2023-01-02|150|800|
|East|Gadget|2023-01-03|100|800|
|East|Widget|2023-01-04|250|800|
|West|Widget|2023-01-01|300|750|
|West|Gadget|2023-01-02|400|750|
|West|Widget|2023-01-03|50|750|

- Total for East partition = 100 + 200 + 150 + 100 + 250 = 800
- Total for West partition = 300 + 400 + 50 = 750

---

## 4. PARTITION BY + ORDER BY in the Window

**Goal**: Demonstrate how rows are processed in a specified order for the window function.

**Query**:

```sql
SELECT
    region,
    sales_date,
    sales_amt,
    SUM(sales_amt) OVER (
        PARTITION BY region
        ORDER BY sales_date
    ) AS running_total
FROM sales
ORDER BY region, sales_date, id;
```

Explanation:

- `ORDER BY sales_date` **within** the window function means we accumulate a running total in date order, but **within each region**.
- Because it’s a **window** definition with `ORDER BY`, PostgreSQL’s default frame is `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`.
- That means the sum includes all rows from the start of the partition up to the current row’s last peer in terms of `sales_date`.

**Result (excerpt)** (focusing on East region first):

|region|sales_date|sales_amt|running_total|
|---|---|---|---|
|East|2023-01-01|100|100|
|East|2023-01-02|200|450|
|East|2023-01-02|150|450|
|East|2023-01-03|100|550|
|East|2023-01-04|250|800|
|West|2023-01-01|300|300|
|West|2023-01-02|400|700|
|West|2023-01-03|50|750|

Note how on `2023-01-02` for East, both rows with `sales_amt=200` and `150` are considered the same “peer group” (same `sales_date`), so the running total after both is 450.

---

## 5. Using a Named Window Specification

**Goal**: Show the difference between referencing a named window vs. defining it inline.

You can define a named window in the `WINDOW` clause, then refer to it by name in `OVER window_name`.

**Query**:

```sql
SELECT
    region,
    sales_date,
    sales_amt,
    SUM(sales_amt) OVER w AS running_total_by_region
FROM sales
WINDOW w AS (
    PARTITION BY region
    ORDER BY sales_date
)
ORDER BY region, sales_date, id;
```

- `WINDOW w AS (...)` is essentially the same as putting `OVER (PARTITION BY region ORDER BY sales_date)` inline, but it can simplify queries if you want to reuse the same window specification multiple times in a single SELECT.

---

## 6. Frame Clauses (ROWS vs RANGE vs GROUPS)

### 6.1 ROWS Example: Rolling 2-Day Window (by rows count)

**Goal**: Demonstrate `ROWS BETWEEN ... AND ...`.

**Query**:

```sql
SELECT
    region,
    sales_date,
    sales_amt,
    SUM(sales_amt) OVER (
        PARTITION BY region
        ORDER BY sales_date
        ROWS BETWEEN 1 PRECEDING AND CURRENT ROW
    ) AS rolling_2_rows
FROM sales
ORDER BY region, sales_date, id;
```

Explanation:

- `ROWS BETWEEN 1 PRECEDING AND CURRENT ROW` means: for each row, look at the current row and the **immediately preceding** row **in the sorted list** (within the region).
- This is purely row-based, **not** date-based. It will sum up to 2 rows at a time, in ascending `sales_date` order.

### 6.2 RANGE Example: Rolling 2-Day Range

Let’s say we want to sum over a sliding 2-day period up to the current date. For that, we might do:

```sql
SELECT
    region,
    sales_date,
    sales_amt,
    SUM(sales_amt) OVER (
        PARTITION BY region
        ORDER BY sales_date
        RANGE BETWEEN '2 days' PRECEDING AND CURRENT ROW
    ) AS rolling_2day_sum
FROM sales
ORDER BY region, sales_date, id;
```

Explanation:

- `RANGE` in PostgreSQL requires the `ORDER BY` to have exactly one column. Here, it’s `sales_date`.
- `'2 days' PRECEDING` means all rows within 2 calendar days before `sales_date` (inclusive) plus the current row’s date.
- If `sales_date` is `2023-01-03`, it includes rows from `2023-01-01`, `2023-01-02`, and `2023-01-03` (since all are within 2 days prior to Jan 3).
- Notice that if multiple rows have the **same** `sales_date`, they are considered peers. In `RANGE` mode, the frame can include all rows with dates that lie within the range, not necessarily just 2 rows.

### 6.3 GROUPS Example: Summing Over 1 Peer Group Before to Current Group

**Goal**: `GROUPS` frame focuses on “peer groups” (rows that tie in the `ORDER BY`).

```sql
SELECT
    region,
    sales_date,
    sales_amt,
    SUM(sales_amt) OVER (
        PARTITION BY region
        ORDER BY sales_date
        GROUPS BETWEEN 1 PRECEDING AND CURRENT ROW
    ) AS sum_across_2_groups
FROM sales
ORDER BY region, sales_date, id;
```

Explanation:

- A “peer group” is a set of rows that share the same ordering value (`sales_date` in this case).
- `GROUPS BETWEEN 1 PRECEDING AND CURRENT ROW` means: take the current row’s peer group (i.e., its date) plus the previous date’s peer group.
- For region = East on `2023-01-02`, that group includes both (sales_date = `2023-01-02`) rows plus the peer group for `2023-01-01`.

---

## 7. Frame Exclusions

**Goal**: Demonstrate how to exclude certain rows from the current frame.

For instance, `EXCLUDE TIES` excludes all peer rows that have the same ordering value as the current row, **except** the current row itself. Meanwhile, `EXCLUDE CURRENT ROW` excludes just the row that is being processed.

Let’s illustrate a scenario with `EXCLUDE CURRENT ROW`:

```sql
SELECT
    region,
    sales_date,
    sales_amt,
    SUM(sales_amt) OVER (
        PARTITION BY region
        ORDER BY sales_date
        RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
        EXCLUDE CURRENT ROW
    ) AS running_total_excl_current
FROM sales
ORDER BY region, sales_date, id;
```

Explanation:

- Normally, `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` means sum from the start of the region’s partition up to the current date’s last peer row (which includes the current row).
- `EXCLUDE CURRENT ROW` removes the current row from that sum. So this column effectively shows the “running total **up to but not including** this row.”

---

## 8. FILTER Clause

**Goal**: Show how `FILTER (WHERE ...)` can pick which rows to feed into the window function.

### 8.1 Filter with an Aggregate Window Function

Example: We want the running sum of **just** `Widget` sales, even though we’re returning rows for all products:

```sql
SELECT
    region,
    product,
    sales_date,
    sales_amt,
    SUM(sales_amt) FILTER (WHERE product = 'Widget')
        OVER (PARTITION BY region ORDER BY sales_date) AS running_widget_sum
FROM sales
ORDER BY region, sales_date, id;
```

Explanation:

- Only rows where `product = 'Widget'` are included in the `SUM(...)`.
- The partition and ordering are the same, but `Gadget` rows effectively contribute `0` to the window function (they are not “fed” to the aggregator).

---

## Putting It All Together

You can combine multiple window functions in the same SELECT, each with different frames or filters. For example:

```sql
SELECT
    region,
    product,
    sales_date,
    sales_amt,
    SUM(sales_amt) OVER (
        PARTITION BY region
        ORDER BY sales_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS cume_sales_by_region,

    SUM(sales_amt) FILTER (WHERE product = 'Widget')
    OVER (PARTITION BY region ORDER BY sales_date) AS running_widget_only,

    COUNT(*) OVER (PARTITION BY region) AS items_in_region
FROM sales
ORDER BY region, sales_date, id;
```

Here, in one pass:

1. `cume_sales_by_region`: running total of `sales_amt` by region, from the start to current row (row-based).
2. `running_widget_only`: running total of only `Widget` sales using `FILTER(...)`.
3. `COUNT(*) OVER (PARTITION BY region)`: counts how many rows are in each region’s partition (repeats the same count on every row in that partition).

---

## Key Takeaways

1. **Window function structure**: `function(args) [FILTER (WHERE ...)] OVER ( [PARTITION BY ...] [ORDER BY ...] [frame_clause] )`.
2. **PARTITION BY**: splits the data set into groups (partitions) within which the window function is independently applied.
3. **ORDER BY** in `OVER` is used for ordering within each partition. Without it, the concept of “current row” or “peers” is less meaningful.
4. **Frames** (`ROWS`, `RANGE`, `GROUPS`) define exactly **which** rows in the partition are considered by the function for each row’s calculation.
    - **ROWS** is about physical row counts.
    - **RANGE** is about value ranges (e.g., time or numeric).
    - **GROUPS** is about peer groups that share the same ordering value.
5. **Frame exclusions** let you remove the current row (or its peers) from the frame.
6. **FILTER** is an optional clause that can be used with aggregate window functions to include/exclude rows based on a condition.

These examples should give you a broad understanding of how to construct increasingly complex window function queries in PostgreSQL. Use them as building blocks for your own analytics use cases.****