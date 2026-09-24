# Sub Queries
Subqueries are SQL queries embedded within another query. They can be used in various parts of SQL statements, such as SELECT, FROM, WHERE, and HAVING clauses.

Also known as nested queries or inner queries.

Subqueries allow for complex data retrieval and manipulation by breaking down complex queries into more manageable parts. 

## Types of Sub Queries

## Nested SubQuery
Nested subqueries are queries embedded within another SQL query. Think of it as a query inside a query, the outer query depends on the result returned by the inner query to complete its own operation.

### Example
```sql
SELECT employee_id,
       employee_name,
       salary
FROM employees
WHERE salary >
(
    SELECT AVG(salary)
    FROM employees
);
```
### How It Works
1. The inner query executes first.
2. It calculates the average salary of all employees.
3. The result is returned to the outer query.
4. The outer query retrieves employees whose salary is greater than the calculated average.

Since the inner query does not reference any column from the outer query, it executes only once and is therefore a nested (non-correlated) subquery.

## Correlated SubQuery
A **correlated subquery** is a subquery that depends on the outer query for its execution. It references one or more columns from the outer query, which means it cannot be executed independently.

Unlike a regular subquery (nested), which is executed once and its result is passed to the outer query, a correlated subquery is executed **once for every row processed by the outer query**.

The subquery is called *correlated* because it is linked to the outer query through the columns it references.

Commonly used in `WHERE`, `SELECT`, and `HAVING` clauses.

### Example

```sql
SELECT e.employee_id,
       e.employee_name,
       e.salary
FROM employees e
WHERE e.salary >
(
    SELECT AVG(salary)
    FROM employees
    WHERE department_id = e.department_id
);
```

### How It Works

For each employee:

1. The outer query picks a row from `employees`.
2. The subquery calculates the average salary of that employee's department.
3. The employee's salary is compared with the calculated average.
4. If the salary is greater than the department average, the row is returned.

Since the subquery uses `e.department_id` from the outer query, it must be executed separately for each employee row, making it a correlated subquery.
