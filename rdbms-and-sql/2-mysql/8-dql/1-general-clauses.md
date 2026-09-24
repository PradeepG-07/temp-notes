# DQL Commands

DQL (**Data Query Language**) is used to retrieve and analyze data from a database. The primary DQL statement is `SELECT`, which can be combined with various clauses to filter, group, sort, and limit results.

### SELECT

The `SELECT` statement is used to retrieve data from one or more tables.

### DISTINCT

The `DISTINCT` keyword removes duplicate rows from the result set.

### WHERE

The `WHERE` clause filters individual rows before grouping and aggregation.

### ORDER BY

The ORDER BY clause sorts the result set in ascending (ASC) or descending (DESC) order.

`ASC` is the default sorting order.
`DESC` sorts in descending order.

### LIMIT

The `LIMIT` clause restricts the number of rows returned by a query.

### OFFSET

The `OFFSET` clause skips a specified number of rows before returning results. It is commonly used with `LIMIT` for pagination.

### Combined Example

```sql
-- Select columns to return
SELECT DISTINCT department, COUNT(*) AS employee_count

-- Source table
FROM employees

-- Filter rows before grouping
WHERE salary > 50000

-- Group rows by department
GROUP BY department

-- Filter groups after aggregation
HAVING COUNT(*) >= 5

-- Sort the final result
ORDER BY employee_count DESC

-- Return only 10 rows
LIMIT 10

-- Skip the first 20 rows
OFFSET 20;
```
# SQL Logical Query Processing Order

Although SQL queries are written in one order, they are logically processed in a different order.

## Example

```sql
SELECT department, COUNT(*)
FROM employees
WHERE salary > 50000
GROUP BY department
HAVING COUNT(*) >= 5
ORDER BY COUNT(*) DESC
LIMIT 10;
```

## Logical Execution Order

### 1. FROM

Identify the source table(s).

```sql
FROM employees
```

### 2. WHERE

Filter rows.

```sql
WHERE salary > 50000
```

### 3. GROUP BY

Create groups.

```sql
GROUP BY department
```

### 4. Aggregate Functions

Compute aggregate values.

```sql
COUNT(*)
```

### 5. HAVING

Filter groups.

```sql
HAVING COUNT(*) >= 5
```

### 6. SELECT

Choose columns to return.

```sql
SELECT department, COUNT(*)
```

### 7. ORDER BY

Sort the results.

```sql
ORDER BY COUNT(*) DESC
```

### 8. LIMIT / OFFSET

Restrict the final output.

```sql
LIMIT 10
```

## Summary

| Logical Order | Clause              |
|---------------|---------------------|
| 1             | FROM                |
| 2             | WHERE               |
| 3             | GROUP BY            |
| 4             | Aggregate Functions |
| 5             | HAVING              |
| 6             | SELECT              |
| 7             | ORDER BY            |
| 8             | LIMIT / OFFSET      |

This logical order explains why aggregate functions cannot be used in the `WHERE` clause and why `HAVING` exists.
