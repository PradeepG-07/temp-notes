# Common Table Expressions (CTE)

## What is a CTE?

A **Common Table Expression (CTE)** is a temporary named result set that exists only for the duration of a query.

It allows you to write a query once, give it a name, and then use it like a table within the main query.

CTEs are primarily used to:

* Improve query readability, maintainability.
* Break complex queries into smaller logical parts.
* Avoid repeating the same subquery multiple times.
* Write recursive queries.

A CTE is defined using the `WITH` keyword.

**Syntax**

```sql
WITH cte_name AS
(
    query
)
SELECT *
FROM cte_name;
```

**Simple Example**

```sql id="x4n7pt"
WITH high_salary_employees AS
(
    SELECT employee_id,
           employee_name,
           salary
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary_employees;
```

The CTE result behaves like a temporary table for the duration of the query.

## Why Use a CTE?
Readability, Maintainability, Reusability, Query Once
```sql
-- Without a CTE:
SELECT *
FROM
(
    SELECT employee_id,
           employee_name,
           salary
    FROM employees
    WHERE salary > 50000
) temp;


-- With a CTE:
WITH high_salary_employees AS
(
    SELECT employee_id,
           employee_name,
           salary
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary_employees;
```

The CTE version is usually easier to read and maintain.

## Multiple CTEs
Multiple CTEs can be defined in a single query.

```sql
WITH high_salary AS
(
    SELECT *
    FROM employees
    WHERE salary > 50000
),
engineering_employees AS
(
    SELECT *
    FROM employees
    WHERE department_id = 10
)
SELECT *
FROM high_salary;
```

## CTE Referencing Another CTE
A CTE can use a previously defined CTE.

```sql
WITH employee_data AS
(
    SELECT *
    FROM employees
),
high_salary AS
(
    SELECT *
    FROM employee_data
    WHERE salary > 50000
)
SELECT *
FROM high_salary;
```

## CTE vs Subquery

| Feature         | CTE                              | Subquery                     |
|-----------------|----------------------------------|------------------------------|
| Readability     | Better                           | Can become difficult to read |
| Reusability     | Can be referenced multiple times | Usually repeated             |
| Naming          | Named result set                 | Anonymous                    |
| Complex Queries | Easier to manage                 | Harder to manage             |

## Recursive CTE

A recursive CTE references itself.

Used for:

* Employee-manager hierarchies
* Organization charts
* Folder structures
* Tree traversal
* Graph traversal

### Syntax

```sql
WITH RECURSIVE cte_name AS
(
    base_query

    UNION ALL

    recursive_query
)
SELECT *
FROM cte_name;
```

## Example: Numbers 1 to 5

```sql
WITH RECURSIVE numbers AS
(
    SELECT 1 AS n

    UNION ALL

    SELECT n + 1
    FROM numbers
    WHERE n < 5
)
SELECT *
FROM numbers;
```

Output:

```text
1
2
3
4
5
```