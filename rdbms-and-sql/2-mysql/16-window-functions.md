# Window Functions

Window functions perform calculations on a group of related rows while still returning every individual row in the result.

Unlike aggregate functions such as `SUM()`, `AVG()`, or `COUNT()`, which combine multiple rows into a single result, window functions preserve all rows and simply add the calculated value to each row.

A window function uses the `OVER()` clause to define the set of rows (called a **window**) on which the calculation should be performed.

### Example

Consider the following data:

| employee_name | salary |
|---------------|--------|
| Alice         | 50000  |
| Bob           | 70000  |
| Charlie       | 70000  |
| David         | 90000  |

Using an aggregate function:

```sql id="7ev5o4"
SELECT AVG(salary)
FROM employees;
```

Output:

```text
70000
```

Only one row is returned because aggregate functions combine multiple rows into a single result.

Using a window function:

```sql
SELECT employee_name,
       salary,
       AVG(salary) OVER() AS avg_salary
FROM employees;
```

Output:

```text
Alice    50000   70000
Bob      70000   70000
Charlie  70000   70000
David    90000   70000
```

All rows are preserved, and the average salary is added to each row.

### Key Characteristics

* Operate on a set of related rows called a **window**.
* Do not collapse multiple rows into a single row.
* Return one result for each row.
* Use the `OVER()` clause.
* Commonly used for ranking, running totals, moving averages, and comparing rows.

### Interview Definition

> A window function performs calculations across a set of related rows while preserving every row in the result set. Unlike aggregate functions, it does not group rows into a single output row. Window functions use the `OVER()` clause to define the window of rows on which the calculation is performed.

## Window Functions
| Function        | Purpose                                         | Common Syntax Variants                     |
|-----------------|-------------------------------------------------|--------------------------------------------|
| `ROW_NUMBER()`  | Assigns a unique sequential number to each row. | `ROW_NUMBER() OVER(...)`                   |
| `RANK()`        | Assigns a rank with gaps for ties.              | `RANK() OVER(...)`                         |
| `DENSE_RANK()`  | Assigns a rank without gaps for ties.           | `DENSE_RANK() OVER(...)`                   |
| `LAG()`         | Returns a value from a previous row.            | `LAG(col) OVER(...)`, `LAG(col, offset)`   |
| `LEAD()`        | Returns a value from a following row.           | `LEAD(col) OVER(...)`, `LEAD(col, offset)` |
| `FIRST_VALUE()` | Returns the first value in the window.          | `FIRST_VALUE(col) OVER(...)`               |
| `LAST_VALUE()`  | Returns the last value in the window.           | `LAST_VALUE(col) OVER(...)`                |

## Sample Data

| employee_id | employee_name | salary |
|-------------|---------------|--------|
| 1           | Alice         | 50000  |
| 2           | Bob           | 70000  |
| 3           | Charlie       | 70000  |
| 4           | David         | 90000  |


## Examples

```sql
-- ROW_NUMBER
SELECT employee_name,
       salary,
       ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num
FROM employees;

-- Output:
-- David   90000   1
-- Bob     70000   2
-- Charlie 70000   3
-- Alice   50000   4


-- RANK
SELECT employee_name,
       salary,
       RANK() OVER (ORDER BY salary DESC) AS rank_num
FROM employees;

-- Output:
-- David   90000   1
-- Bob     70000   2
-- Charlie 70000   2
-- Alice   50000   4


-- DENSE_RANK
SELECT employee_name,
       salary,
       DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank_num
FROM employees;

-- Output:
-- David   90000   1
-- Bob     70000   2
-- Charlie 70000   2
-- Alice   50000   3


-- LAG
SELECT employee_name,
       salary,
       LAG(salary) OVER (ORDER BY salary) AS previous_salary
FROM employees;

-- Output:
-- Alice   50000   NULL
-- Bob     70000   50000
-- Charlie 70000   70000
-- David   90000   70000


-- LEAD
SELECT employee_name,
       salary,
       LEAD(salary) OVER (ORDER BY salary) AS next_salary
FROM employees;

-- Output:
-- Alice   50000   70000
-- Bob     70000   70000
-- Charlie 70000   90000
-- David   90000   NULL


-- FIRST_VALUE
SELECT employee_name,
       salary,
       FIRST_VALUE(salary)
       OVER (ORDER BY salary) AS lowest_salary
FROM employees;

-- Output:
-- Alice   50000   50000
-- Bob     70000   50000
-- Charlie 70000   50000
-- David   90000   50000


-- LAST_VALUE
SELECT employee_name,
       salary,
       LAST_VALUE(salary)
       OVER (
           ORDER BY salary
           ROWS BETWEEN UNBOUNDED PRECEDING
           AND UNBOUNDED FOLLOWING
       ) AS highest_salary
FROM employees;

-- Output:
-- Alice   50000   90000
-- Bob     70000   90000
-- Charlie 70000   90000
-- David   90000   90000
```

## PARTITION BY

`PARTITION BY` divides rows into groups before applying the window function.

```sql
SELECT employee_name,
       department_id,
       salary,
       RANK() OVER (
           PARTITION BY department_id
           ORDER BY salary DESC
       ) AS dept_rank
FROM employees;
```

Each department gets its own ranking.

---

### Difference Between Aggregate and Window Functions

| Aggregate Function        | Window Function  |
| ------------------------- | ---------------- |
| Returns one row per group | Returns all rows |
| Uses GROUP BY             | Uses OVER()      |
| Collapses rows            | Preserves rows   |

Example:

```sql id="u7g5es"
AVG(salary)
```

returns one value.

```sql id="m2c8kw"
AVG(salary) OVER()
```

returns the average salary on every row while keeping all rows visible.
