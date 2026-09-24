# Operators
Mainly, there are six types of operators.
1. Arithmetic Operators
2. Comparison Operators
3. Logical Operators
4. Special Operators (LIKE, IN, BETWEEN, ANY, ALL)
5. Set Operators
6. Bitwise Operators


# Arithmetic Operators
Mainly used to perform calculations. We have addition(+), subtraction(-), multiplication(*), division(/), modulo(%).

### Operator Precedence Hierarchy (Highest to Lowest):
- Parentheses ()
- Exponentiation (^)
- Multiplication (*), Division (/), and Modulo (%)
- Addition (+) and Subtraction (-)

Mostly used in `SELECT` statements.
```sql
SELECT col_name (operator) (col_name) FROM table_name;
```
# Comparison Operators
Mainly used to determine relationships in the `WHERE` or `HAVING` clauses. Results in `boolean` value.
We have equals(`=`), not equals(`<>` or `!=`), greaterthan(`>`), greaterthanorequal(`>=`), lessthan(`<`) and lessthanorequal(`<=`).

Handling `NULL` values is special here. We have `IS NULL` and `IS NOT NULL` to perform equality and inequality for `NULL` values.

# Logical Operators
Logical operators in SQL are used to combine multiple conditions in a query and evaluate them as a single Boolean result (True or False). We have `AND`, `OR`, and `NOT` operators.

### Logical Operator Precedence Hierarchy:
1. NOT 
2. AND
3. OR

# Special Operators
## LIKE Operator
`LIKE` operator is particularly useful when dealing with text data that follows a particular format or contains specific characters.

The LIKE operator searches for matches in the specified column based on the pattern provided.

### Wildcard characters (% and _)
The `LIKE` operator supports two wildcard characters:
1. **Percent Sign (%)**: It represents zero, one, or multiple characters. When used at the beginning of a pattern, it matches any sequence of characters. When used at the end, it matches any characters followed by the specified sequence.
2. **Underscore (_)**: It represents a single character. When used in a pattern, it matches any single character in that position.

### Examples
```sql
-- Retrieving all employees whose last name ends with “son”:
SELECT *
FROM employees
WHERE last_name LIKE '%son';

-- Retrieving all employees whose last name start with “son”:
SELECT *
FROM employees
WHERE last_name LIKE 'son%';

-- Retrieving all products whose names contain “laptop” anywhere in the string:
SELECT *
FROM products
WHERE product_name LIKE '%laptop%';
```

## IN Operator
IN Operator allows users to specify a list of values, and the operator checks if a given expression matches any of the values in the list.
```sql
SELECT *
FROM orders
WHERE status IN ('Shipped', 'Delivered', 'Out for Delivery');
```

### Performance Benefits
The `IN` operator can offer **performance benefits over multiple `OR` conditions**, especially when dealing with large datasets. The database optimizer can efficiently process the `IN` operator, resulting in faster query execution.

## BETWEEN Operator
`BETWEEN` Operator allows users to specify a range of values, and the operator checks if a given expression falls within that range. 

The `BETWEEN` operator is particularly useful when dealing with date ranges, numeric intervals, or filtering data based on specific conditions.

The BETWEEN operator will return true if the column_name falls within the specified range, **inclusive of the boundaries.**

### Performance Benefits
The `BETWEEN` operator can offer **performance benefits over using multiple comparison operators**, especially when dealing with large datasets. The database optimizer can efficiently process the `BETWEEN` operator, resulting in faster query execution.

```sql
SELECT *
FROM employees
WHERE years_of_service BETWEEN 5 AND 10;

SELECT *
FROM orders
WHERE order_date BETWEEN '2023-01-01' AND '2023-06-30';
```

## EXISTS Operator

The `EXISTS` operator checks whether a subquery returns at least one row.

### Characteristics
- Returns TRUE if the subquery returns one or more rows.
- Returns FALSE if the subquery returns no rows.
- Commonly used with correlated subqueries.

```sql
SELECT *
FROM customers c
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```

## ANY Operator
The `ANY` operator compares a value with any value returned by a subquery.

### Characteristics
- Condition becomes TRUE if at least one comparison is TRUE.

```sql
SELECT *
FROM employees
WHERE salary > ANY (
    SELECT salary
    FROM employees
    WHERE department_id = 10
);
```
Equivalent meaning:
- Salary is greater than at least one salary in department 10.

## ALL Operator

The `ALL` operator compares a value with all values returned by a subquery.

### Characteristics
- Condition becomes TRUE only if all comparisons are TRUE.

```sql
SELECT *
FROM employees
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department_id = 10
);
```
Equivalent meaning:
- Salary is greater than every salary in department 10.

# Set Operators
Set operators are used to combine the result sets of two or more `SELECT` statements.

### Rules for Set Operators
1. Both queries must return the same number of columns.
2. Corresponding columns must have compatible data types.
3. Column names in the final result are taken from the first query.

## UNION
Combines the results of two queries and removes duplicate rows.

```sql
SELECT city
FROM customers

UNION

SELECT city
FROM suppliers;
```

### Characteristics
- Removes duplicates.
- Requires additional work to eliminate duplicate records.
- Generally slower than `UNION ALL`.

## UNION ALL
Combines the results of two queries and keeps duplicate rows.

```sql
SELECT city
FROM customers

UNION ALL

SELECT city
FROM suppliers;
```

### Characteristics
- Does not remove duplicates.
- Faster than `UNION`.
- Use when duplicates are acceptable or desired.

## INTERSECT
Returns only the rows that exist in both result sets.

```sql
SELECT customer_id
FROM orders_2024

INTERSECT

SELECT customer_id
FROM orders_2025;
```

### Characteristics
- Returns common rows.
- Duplicate rows are removed.

## EXCEPT
Returns rows from the first query that do not exist in the second query.

```sql
SELECT customer_id
FROM customers

EXCEPT

SELECT customer_id
FROM inactive_customers;
```
### Characteristics
- Performs set subtraction.
- Duplicate rows are removed.
- Order matters.

# Bitwise Operators
Bitwise operators perform operations on the individual bits of integer values.

Before performing the operation, numbers are converted to their binary representation.

```sql
-- Bitwise AND (&)
SELECT 5 & 3;      -- 5 & 3 = 1

-- Bitwise OR (|)
SELECT 5 | 3;      -- 5 | 3 = 7

-- Bitwise XOR (^)
SELECT 5 ^ 3;      -- 5 ^ 3 = 6

-- Bitwise NOT (~)
SELECT ~5;         -- ~5 = -6

-- Left Shift (<<)
SELECT 5 << 1;     -- 5 << 1 = 10

SELECT 5 << 2;     -- 5 << 2 = 20

-- Right Shift (>>)
SELECT 20 >> 1;    -- 20 >> 1 = 10

SELECT 20 >> 2;    -- 20 >> 2 = 5
```