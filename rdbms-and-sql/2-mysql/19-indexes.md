# Indexes

## What is an Index?

An **Index** is a database object that improves the speed of data retrieval operations.

Without an index, the database may need to scan every row in a table to find matching data (**Full Table Scan**).

An index creates a separate data structure that allows the database to locate rows much faster.

Think of an index like the index of a book:

```text
Without Index:
Read every page until you find the topic.

With Index:
Look up the topic in the index and jump directly to the page.
```

## Why Do We Need Indexes?

Consider a table with 10 million employees.

```sql
SELECT *
FROM employees
WHERE employee_id = 1001;
```

Without an index:

```text
Database scans all rows.
Time Complexity ≈ O(n)
```

With an index:

```text
Database directly navigates to the required row.
Time Complexity ≈ O(log n)
```

## How Indexes Work

Most MySQL indexes use a **B-Tree (Balanced Tree)** data structure.

Example:

```text
                 50
              /      \
           20          80
         /   \       /    \
       10    30    70     90
```

Searching:

```text
Find 70

50 -> go right
80 -> go left
70 -> found
```

The database does not need to scan every row.

### Create Index
```sql
CREATE INDEX idx_employee_name
ON employees(employee_name);
```

### Drop Index

```sql
DROP INDEX idx_employee_name
ON employees;
```

### View Indexes

```sql
SHOW INDEXES
FROM employees;
```

## Types of Indexes

### Primary Index

Created automatically when a `PRIMARY KEY` is defined.

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100)
);
```

Characteristics:

* Unique
* Cannot be NULL
* One per table

### Unique Index
Ensures duplicate values cannot exist.

A UNIQUE index or constraint allows multiple NULL values in MySQL because NULL represents an unknown value. Since SQL treats NULL as not equal to any value, including another NULL, multiple NULLs do not violate the uniqueness requirement.
```sql
CREATE UNIQUE INDEX idx_email
ON employees(email);
```

### Single-Column Index

Created on a single column.

```sql
CREATE INDEX idx_name
ON employees(employee_name);
```

### Composite Index (Multi-Column Index)

Created on multiple columns.

```sql
CREATE INDEX idx_dept_salary
ON employees(department_id, salary);
```
Index order matters.

## Leftmost Prefix Rule

For index:

```sql
(department_id, salary, joining_date)
```

Works efficiently:

```sql
department_id

department_id, salary

department_id, salary, joining_date
```

Not efficient:

```sql
salary

joining_date

salary, joining_date
```

because the leftmost column is skipped.


### Full-Text Index

Used for text searching.

```sql
CREATE FULLTEXT INDEX idx_description
ON products(description);
```

Example:

```sql
SELECT *
FROM products
WHERE MATCH(description)
AGAINST('laptop');
```

### Clustered Index

The actual table data is physically stored in the same order as the index.

In MySQL InnoDB:

```text
PRIMARY KEY = Clustered Index
```

Example:

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY
);
```

Data is stored ordered by `employee_id`.

Characteristics:

* Only one clustered index per table.
* Fast range queries.
* Usually the primary key.

### Non-Clustered Index

Stores only:

```text
Indexed Value
+
Pointer to Actual Row
```

Example:

```sql
CREATE INDEX idx_name
ON employees(employee_name);
```

Characteristics:

* Multiple non-clustered indexes allowed.
* Requires an extra lookup to fetch row data.

### Covering Index

A covering index contains all columns needed by a query. Hence, the database can satisfy the query using only the index. Therefore, No table lookup is required.

This improves performance.

Example:

```sql
-- Index:
CREATE INDEX idx_cover
ON employees(department_id, salary);

-- Query:
SELECT department_id, salary
FROM employees
WHERE department_id = 10;
```

### When Should We Create an Index?

```text
PRIMARY KEY columns
FOREIGN KEY columns
WHERE clause columns
JOIN columns
ORDER BY columns
GROUP BY columns
```

### When Should We Avoid Indexes?

```text
Very small tables
Columns with frequent updates
Columns with very low uniqueness
```

## Advantages and Disadvantages of Indexes
Faster SELECT queries, JOIN operations, sorting and filtering

1. Consumes storage
2. Slower INSERT, UPDATE, DELETE because every index must also be updated whenever data changes. 
3. Additional maintenance cost

## EXPLAIN
Used to analyze query execution plans.

```sql
EXPLAIN
SELECT *
FROM employees
WHERE employee_id = 1001;
```

Useful for checking Index Usage, Table Scans, Join Strategy, Estimated Rows

Running EXPLAIN does not actually execute the query. It just creates and shows the query plan and metrics.





## Most Important for Interviews

```sql
PRIMARY KEY Index
UNIQUE Index
Composite Index
Clustered Index
Non-Clustered Index
Covering Index
EXPLAIN
```
