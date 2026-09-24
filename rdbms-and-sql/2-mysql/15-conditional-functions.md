# Conditional Functions

Conditional functions are used to perform decision-making, handle NULL values, and return different results based on conditions.

| Function     | Purpose                                                             | Common Syntax Variants                                |
|--------------|---------------------------------------------------------------------|-------------------------------------------------------|
| `IF()`       | Returns one value if a condition is true and another if false.      | `IF(condition, true_value, false_value)`              |
| `CASE`       | Evaluates multiple conditions and returns the corresponding result. | `CASE WHEN condition THEN result ... ELSE result END` |
| `IFNULL()`   | Returns an alternate value if the expression is NULL.               | `IFNULL(expression, replacement)`                     |
| `NULLIF()`   | Returns NULL if two expressions are equal.                          | `NULLIF(expr1, expr2)`                                |
| `COALESCE()` | Returns the first non-NULL value.                                   | `COALESCE(val1, val2, ...)`                           |

## Examples

```sql
-- IF
SELECT IF(100 > 50, 'True', 'False');
-- Output: True

SELECT IF(25 > 50, 'Pass', 'Fail');
-- Output: Fail

-- CASE
SELECT CASE
         WHEN 95 >= 90 THEN 'Grade A'
         WHEN 95 >= 75 THEN 'Grade B'
         WHEN 95 >= 60 THEN 'Grade C'
         ELSE 'Grade D'
       END;
-- Output: Grade A

SELECT CASE
         WHEN 45 >= 90 THEN 'Grade A'
         WHEN 45 >= 75 THEN 'Grade B'
         WHEN 45 >= 60 THEN 'Grade C'
         ELSE 'Grade D'
       END;
-- Output: Grade D

-- IFNULL
SELECT IFNULL(NULL, 'Not Available');
-- Output: Not Available

SELECT IFNULL('John', 'Not Available');
-- Output: John

-- NULLIF
SELECT NULLIF(100, 100);
-- Output: NULL

SELECT NULLIF(100, 200);
-- Output: 100

-- COALESCE
SELECT COALESCE(NULL, NULL, 'John', 'Doe');
-- Output: John

SELECT COALESCE(NULL, NULL, NULL, 'Default');
-- Output: Default

SELECT COALESCE('Email', 'Phone', 'Address');
-- Output: Email
```

## Real-World Examples

```sql
-- Categorize salaries
SELECT employee_name,
       salary,
       CASE
           WHEN salary >= 100000 THEN 'Senior'
           WHEN salary >= 50000 THEN 'Mid'
           ELSE 'Junior'
       END AS level
FROM employees;

-- Handle NULL phone numbers
SELECT employee_name,
       IFNULL(phone_number, 'Not Available')
FROM employees;

-- Get first available contact method
SELECT employee_name,
       COALESCE(phone_number, email, 'No Contact')
FROM employees;
```

## Common Interview Questions

### IF() vs CASE

```sql
SELECT IF(100 > 50, 'Yes', 'No');
-- Output: Yes

SELECT CASE
         WHEN 100 > 50 THEN 'Yes'
         ELSE 'No'
       END;
-- Output: Yes
```

**IF()**

* Simpler.
* Suitable for a single condition.

**CASE**

* Supports multiple conditions.
* More readable for complex business logic.
* ANSI SQL standard.

---

### IFNULL() vs COALESCE()

```sql
SELECT IFNULL(NULL, 'Default');
-- Output: Default

SELECT COALESCE(NULL, NULL, 'Default');
-- Output: Default
```

**IFNULL()**

* Accepts exactly 2 arguments.
* MySQL-specific.

**COALESCE()**

* Accepts multiple arguments.
* ANSI SQL standard.
* Returns the first non-NULL value.

---

### When is COALESCE() useful?

```sql
SELECT COALESCE(phone, email, emergency_contact, 'No Contact');
-- Output: First non-NULL value
```

Commonly used when data may exist in multiple columns.

---

### What does NULLIF() do?

```sql
SELECT NULLIF(10, 10);
-- Output: NULL

SELECT NULLIF(10, 20);
-- Output: 10
```

Equivalent to:

```sql
CASE
    WHEN expr1 = expr2 THEN NULL
    ELSE expr1
END
```

Useful for avoiding division-by-zero errors:

```sql
SELECT salary / NULLIF(employee_count, 0);
```

If `employee_count` is 0, `NULLIF()` returns `NULL` instead of causing an error.
