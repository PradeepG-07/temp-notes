# Aggregate Queries

Aggregate queries in SQL are used to summarize data from multiple rows using aggregate functions.

## Aggregate Functions
Common aggregate functions include:

1. `COUNT()` – Counts rows or non-NULL values.
2. `SUM()` – Returns the total of numeric values.
3. `AVG()` – Returns the average of numeric values.
4. `MIN()` – Returns the smallest value.
5. `MAX()` – Returns the largest value.

> `COUNT()` can be used on any column or with `*`.
>
> `MIN()` and `MAX()` can be used on numeric, string, date, and other comparable data types.
>
> `SUM()` and `AVG()` are used on numeric columns.

Aggregate functions can be used directly or together with the `GROUP BY` clause.

```sql
SELECT SUM(price) FROM orders;
-- Returns the total price of all orders as a single row

SELECT category, SUM(price)
FROM orders
GROUP BY category;
-- Returns the total price for each category
```

## Aggregate Clauses

### GROUP BY

The `GROUP BY` clause is used to group rows that have the same value in one or more columns. Aggregate functions are then calculated separately for each group.

```sql
SELECT category, SUM(price)
FROM orders
GROUP BY category;
```

### HAVING

The `HAVING` clause is used to filter groups after aggregation has been performed.

`WHERE` filters individual rows before grouping, whereas `HAVING` filters the grouped results after aggregation.

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

The above query returns only those departments that contain more than 5 employees.

### WHERE vs HAVING

| Clause   | Filters         | Execution Stage                 |
|----------|-----------------|---------------------------------|
| `WHERE`  | Individual rows | Before grouping and aggregation |
| `HAVING` | Groups          | After grouping and aggregation  |

```sql
SELECT department, COUNT(*)
FROM employees
WHERE salary > 50000
GROUP BY department
HAVING COUNT(*) >= 5;
```

Execution order:

1. `WHERE salary > 50000` filters rows.
2. `GROUP BY department` creates groups.
3. `COUNT(*)` calculates the aggregate value for each group.
4. `HAVING COUNT(*) >= 5` filters the groups.


## Aggregate Concepts

### COUNT(*) vs COUNT(column)

| COUNT(*)                         | COUNT(column)                                        |
|----------------------------------|------------------------------------------------------|
| Counts all rows.                 | Counts only non-NULL values in the specified column. |
| `SELECT COUNT(*)FROM employees;` | `SELECT COUNT(manager_id) FROM employees;`           |


### COUNT(DISTINCT column)

Counts unique non-NULL values.

```sql
SELECT COUNT(DISTINCT department)
FROM employees;
```

### NULL Handling in Aggregate Functions

Consider:

| salary |
|--------|
| 50000  |
| 60000  |
| NULL   |

1. COUNT(*): Returns 3 as output 
2. COUNT(salary): Returns 2 as output

> `NULL` values are ignored by aggregate functions except `COUNT(*)`.

## Aggregate Functions With and Without GROUP BY

```sql
SELECT AVG(salary)
FROM employees;
```
Returns a single summary row.


```sql
SELECT department, AVG(salary)
FROM employees
GROUP BY department;
```
Returns one row per department.

## Multiple-Column GROUP BY

```sql
SELECT department, location, COUNT(*)
FROM employees
GROUP BY department, location;
```
Creates groups using the combination of department and location.

## GROUP BY Rule

Every non-aggregated column in the `SELECT` list must appear in the `GROUP BY` clause.

### Valid

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department;
```

### Invalid

```sql
SELECT department, name, COUNT(*)
FROM employees
GROUP BY department;
```
`name` is neither aggregated nor included in the `GROUP BY` clause.
