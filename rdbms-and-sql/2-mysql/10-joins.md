# SQL Joins

## What are Joins?

Joins are used to combine rows from two or more tables based on a related column between them.

Joins allow data stored across multiple tables to be queried as a single result set.

## Types of Joins

| Join Type         | Description                                                                  |
|-------------------|------------------------------------------------------------------------------|
| `INNER JOIN`      | Returns matching rows from both tables.                                      |
| `LEFT JOIN`       | Returns all rows from the left table and matching rows from the right table. |
| `RIGHT JOIN`      | Returns all rows from the right table and matching rows from the left table. |
| `FULL OUTER JOIN` | Returns all matching and non-matching rows from both tables.                 |
| `CROSS JOIN`      | Returns the Cartesian product of both tables.                                |
| `SELF JOIN`       | Joins a table with itself.                                                   |

## Sample Tables

### Employees

| employee_id | name    | department_id |
|-------------|---------|---------------|
| 1           | Alice   | 10            |
| 2           | Bob     | 20            |
| 3           | Charlie | 30            |
| 4           | David   | NULL          |

### Departments

| department_id | department_name |
|---------------|-----------------|
| 10            | HR              |
| 20            | Finance         |
| 40            | Sales           |


### INNER JOIN
Most commonly used join. Returns only the rows that have matching values in both tables.

`INNER JOIN` or `JOIN` can be used interchangeably.

Syntax
```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

Example
```sql
SELECT e.employee_id,
       e.name,
       d.department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id;
```

Result

| employee_id | name  | department_name |
|-------------|-------|-----------------|
| 1           | Alice | HR              |
| 2           | Bob   | Finance         |


### LEFT JOIN

Returns all rows from the left table and matching rows from the right table.

If no match exists, columns from the right table contain `NULL`.

Syntax

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```

Example

```sql
SELECT e.employee_id,
       e.name,
       d.department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id;
```

Result

| employee_id | name    | department_name |
|-------------|---------|-----------------|
| 1           | Alice   | HR              |
| 2           | Bob     | Finance         |
| 3           | Charlie | NULL            |
| 4           | David   | NULL            |

### RIGHT JOIN

Returns all rows from the right table and matching rows from the left table.

If no match exists, columns from the left table contain `NULL`.

Syntax

```sql
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;
```

Example

```sql
SELECT e.employee_id,
       e.name,
       d.department_name
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.department_id;
```

Result

| employee_id | name  | department_name |
|-------------|-------|-----------------|
| 1           | Alice | HR              |
| 2           | Bob   | Finance         |
| NULL        | NULL  | Sales           |

### FULL OUTER JOIN

Returns:

* Matching rows from both tables.
* Non-matching rows from the left table.
* Non-matching rows from the right table.

Syntax

```sql
SELECT columns
FROM table1
FULL OUTER JOIN table2
ON table1.column = table2.column;
```

### Important MySQL Note

MySQL does **not** support `FULL OUTER JOIN` directly.

It is typically simulated using:

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column

UNION

SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;
```

Result

| employee_id | name    | department_name |
|-------------|---------|-----------------|
| 1           | Alice   | HR              |
| 2           | Bob     | Finance         |
| 3           | Charlie | NULL            |
| 4           | David   | NULL            |
| NULL        | NULL    | Sales           |


### CROSS JOIN
Returns the Cartesian product of two tables.

Every row from the first table is combined with every row from the second table.

No join condition is required. It produces all the combinations between rows of two tables.

Syntax

```sql
SELECT columns
FROM table1
CROSS JOIN table2;
```

Example

```sql
SELECT e.name,
       d.department_name
FROM employees e
CROSS JOIN departments d;
```

Result Count: `rows_in_table1 × rows_in_table2`
Example: `4 employees × 3 departments = 12 rows`

### SELF JOIN

A self join joins a table with itself. This join requires table aliases because the same table participates multiple times in the query and each instance must be distinguished.

It is commonly used when rows in the same table have relationships with other rows in the same table.

Commonly used for hierarchical relationships.

### Example Table

| employee_id | employee_name | manager_id |
|-------------|---------------|------------|
| 1           | Alice         | NULL       |
| 2           | Bob           | 1          |
| 3           | Charlie       | 1          |
| 4           | David         | 2          |

Syntax

```sql
SELECT columns
FROM table_name t1
JOIN table_name t2
ON t1.column = t2.column;
```
Example

```sql
SELECT e.employee_name,
       m.employee_name AS manager_name
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.employee_id;
```
Result

| employee_name | manager_name |
|---------------|--------------|
| Alice         | NULL         |
| Bob           | Alice        |
| Charlie       | Alice        |
| David         | Bob          |
