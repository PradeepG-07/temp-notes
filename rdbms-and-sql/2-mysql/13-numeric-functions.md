# Numeric Functions

| Function               | Purpose                                      | Common Syntax Variants               |
|------------------------|----------------------------------------------|--------------------------------------|
| `ROUND()`              | Rounds a number to the nearest value.        | `ROUND(num)`, `ROUND(num, decimals)` |
| `CEIL()` / `CEILING()` | Rounds a number up to the nearest integer.   | `CEIL(num)`, `CEILING(num)`          |
| `FLOOR()`              | Rounds a number down to the nearest integer. | `FLOOR(num)`                         |
| `ABS()`                | Returns the absolute value of a number.      | `ABS(num)`                           |
| `MOD()`                | Returns the remainder of a division.         | `MOD(a, b)`, `a % b`                 |
| `POWER()`              | Raises a number to a specified power.        | `POWER(base, exponent)`              |
| `SQRT()`               | Returns the square root of a number.         | `SQRT(num)`                          |
| `RAND()`               | Generates a random number between 0 and 1.   | `RAND()`, `RAND(seed)`               |
| `SIGN()`               | Returns the sign of a number.                | `SIGN(num)`                          |
| `GREATEST()`           | Returns the largest value from a list.       | `GREATEST(val1, val2, ...)`          |
| `LEAST()`              | Returns the smallest value from a list.      | `LEAST(val1, val2, ...)`             |

## Examples

```sql
-- ROUND
SELECT ROUND(15.67);
-- Output: 16

SELECT ROUND(15.6789, 2);
-- Output: 15.68

-- CEIL
SELECT CEIL(15.01);
-- Output: 16

-- CEILING
SELECT CEILING(15.01);
-- Output: 16

-- FLOOR
SELECT FLOOR(15.99);
-- Output: 15

-- ABS
SELECT ABS(-100);
-- Output: 100

-- MOD
SELECT MOD(10, 3);
-- Output: 1

SELECT 10 % 3;
-- Output: 1

-- POWER
SELECT POWER(2, 4);
-- Output: 16

-- SQRT
SELECT SQRT(81);
-- Output: 9

-- RAND
SELECT RAND();
-- Output: Random value between 0 and 1
-- Example: 0.734521

SELECT RAND(10);
-- Output: Deterministic random value based on seed

-- SIGN
SELECT SIGN(100);
-- Output: 1

SELECT SIGN(-100);
-- Output: -1

SELECT SIGN(0);
-- Output: 0

-- GREATEST
SELECT GREATEST(10, 20, 5, 15);
-- Output: 20

-- LEAST
SELECT LEAST(10, 20, 5, 15);
-- Output: 5
```

`GREATEST()` and `LEAST()` compare values within a single row or expression, whereas `MAX()` and `MIN()` aggregate values across multiple rows.


## Common Interview Questions

### What is the difference between ROUND() and CEIL()?

```sql
SELECT ROUND(10.4);
-- Output: 10

SELECT CEIL(10.4);
-- Output: 11
```

* `ROUND()` rounds to the nearest value.
* `CEIL()` always rounds upward.
