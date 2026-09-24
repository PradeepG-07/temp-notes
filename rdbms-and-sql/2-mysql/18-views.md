# Views

## What is a View?

A **View** is a virtual table whose data is derived from one or more underlying tables.

A view does not usually store data itself. Instead, it stores a SQL query and executes that query whenever the view is accessed.

Views are commonly used to:

* Simplify complex queries.
* Hide sensitive columns.
* Provide a consistent interface to data.
* Improve code reusability.

## Syntax

### Create View

```sql
CREATE VIEW view_name AS
SELECT column1, column2
FROM table_name;
```

### Query View

```sql
SELECT *
FROM view_name;
```

### Drop View

```sql
DROP VIEW view_name;
```

## Example

### Employees Table

| employee_id | employee_name | salary | department |
|-------------|---------------|--------|------------|
| 1           | Alice         | 50000  | HR         |
| 2           | Bob           | 70000  | Finance    |
| 3           | Charlie       | 90000  | IT         |

### Create View

```sql
CREATE VIEW employee_summary AS
SELECT employee_id,
       employee_name,
       department
FROM employees;
```

### Query View

```sql
SELECT *
FROM employee_summary;
```

Output:

```text
1  Alice    HR
2  Bob      Finance
3  Charlie  IT
```

## View Based on Multiple Tables

```sql
CREATE VIEW employee_department_view AS
SELECT e.employee_id,
       e.employee_name,
       d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id;
```

The user can query the view without knowing the underlying join.

## Updating a View

```sql
CREATE OR REPLACE VIEW employee_summary AS
SELECT employee_id,
       employee_name
FROM employees;
```

## Deleting a View

```sql
DROP VIEW employee_summary;
```

## Updatable Views

Some views allow INSERT, UPDATE, and DELETE operations.

Example:

```sql
CREATE VIEW employee_basic AS
SELECT employee_id,
       employee_name
FROM employees;
```

```sql
UPDATE employee_basic
SET employee_name = 'John'
WHERE employee_id = 1;
```

The underlying table is updated.

## Non-Updatable Views

Views generally become non-updatable when they contain:

```sql
GROUP BY
DISTINCT
UNION
Aggregate Functions
Subqueries
Window Functions
```

Example:

```sql
CREATE VIEW department_stats AS
SELECT department_id,
       COUNT(*) AS employee_count
FROM employees
GROUP BY department_id;
```

This view cannot usually be updated.

## Advantages of Views

### Security

Hide sensitive columns.

```sql
CREATE VIEW employee_public AS
SELECT employee_id,
       employee_name
FROM employees;
```

Salary information remains hidden.

### Simplicity

Hide complex joins.

```sql
SELECT *
FROM employee_department_view;
```

### Reusability

Write once, use many times.


## Disadvantages of Views
* Complex views can reduce performance.
* Nested views can become difficult to maintain.
* Views do not automatically improve query speed.

## View vs Table

| Feature            | View       | Table |
|--------------------|------------|-------|
| Stores Data        | Usually No | Yes   |
| Physical Storage   | No         | Yes   |
| Can Contain Joins  | Yes        | N/A   |
| Exists Permanently | Yes        | Yes   |
| Created From Query | Yes        | No    |

## View vs CTE

| View                        | CTE                    |
|-----------------------------|------------------------|
| Permanent database object   | Temporary query object |
| Created once                | Created per query      |
| Reusable across queries     | Only inside one query  |
| Created using `CREATE VIEW` | Created using `WITH`   |
