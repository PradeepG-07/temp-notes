# Date & Time Functions

| Function          | Purpose                                                              | Common Syntax Variants                |
|-------------------|----------------------------------------------------------------------|---------------------------------------|
| `NOW()`           | Returns the current date and time.                                   | `NOW()`                               |
| `CURDATE()`       | Returns the current date.                                            | `CURDATE()`                           |
| `CURTIME()`       | Returns the current time.                                            | `CURTIME()`                           |
| `YEAR()`          | Extracts the year from a date.                                       | `YEAR(date)`                          |
| `MONTH()`         | Extracts the month number from a date.                               | `MONTH(date)`                         |
| `DAY()`           | Extracts the day of the month.                                       | `DAY(date)`                           |
| `DAYNAME()`       | Returns the weekday name.                                            | `DAYNAME(date)`                       |
| `MONTHNAME()`     | Returns the month name.                                              | `MONTHNAME(date)`                     |
| `DATE_ADD()`      | Adds an interval to a date.                                          | `DATE_ADD(date, INTERVAL value unit)` |
| `DATE_SUB()`      | Subtracts an interval from a date.                                   | `DATE_SUB(date, INTERVAL value unit)` |
| `DATEDIFF()`      | Returns difference between two dates in days.                        | `DATEDIFF(date1, date2)`              |
| `TIMESTAMPDIFF()` | Returns difference between two dates/timestamps in a specified unit. | `TIMESTAMPDIFF(unit, start, end)`     |
| `LAST_DAY()`      | Returns the last day of the month.                                   | `LAST_DAY(date)`                      |
| `EXTRACT()`       | Extracts a specific part of a date/time value.                       | `EXTRACT(part FROM date)`             |

## Examples

```sql
-- NOW
SELECT NOW();
-- Output: 2026-09-22 16:30:45
-- (Current date and time)

-- CURDATE
SELECT CURDATE();
-- Output: 2026-09-22
-- (Current date)

-- CURTIME
SELECT CURTIME();
-- Output: 16:30:45
-- (Current time)

-- YEAR
SELECT YEAR('2026-09-22');
-- Output: 2026

-- MONTH
SELECT MONTH('2026-09-22');
-- Output: 9

-- DAY
SELECT DAY('2026-09-22');
-- Output: 22

-- DAYNAME
SELECT DAYNAME('2026-09-22');
-- Output: Tuesday

-- MONTHNAME
SELECT MONTHNAME('2026-09-22');
-- Output: September

-- DATE_ADD
SELECT DATE_ADD('2026-09-22', INTERVAL 10 DAY);
-- Output: 2026-10-02

SELECT DATE_ADD('2026-09-22', INTERVAL 2 MONTH);
-- Output: 2026-11-22

-- DATE_SUB
SELECT DATE_SUB('2026-09-22', INTERVAL 15 DAY);
-- Output: 2026-09-07

SELECT DATE_SUB('2026-09-22', INTERVAL 1 YEAR);
-- Output: 2025-09-22

-- DATEDIFF
SELECT DATEDIFF('2026-09-30', '2026-09-22');
-- Output: 8

-- TIMESTAMPDIFF
SELECT TIMESTAMPDIFF(YEAR, '2000-01-01', '2026-09-22');
-- Output: 26

SELECT TIMESTAMPDIFF(MONTH, '2026-01-01', '2026-09-22');
-- Output: 8

SELECT TIMESTAMPDIFF(DAY, '2026-09-01', '2026-09-22');
-- Output: 21

-- LAST_DAY
SELECT LAST_DAY('2026-02-10');
-- Output: 2026-02-28

SELECT LAST_DAY('2024-02-10');
-- Output: 2024-02-29

-- EXTRACT
SELECT EXTRACT(YEAR FROM '2026-09-22');
-- Output: 2026

SELECT EXTRACT(MONTH FROM '2026-09-22');
-- Output: 9

SELECT EXTRACT(DAY FROM '2026-09-22');
-- Output: 22
```

## Common Interview Questions

### What is the difference between NOW() and CURDATE()?

```sql
SELECT NOW();
-- Output: 2026-09-22 16:30:45

SELECT CURDATE();
-- Output: 2026-09-22
```

* `NOW()` returns both date and time.
* `CURDATE()` returns only the date.


### What is the difference between DATEDIFF() and TIMESTAMPDIFF()?

```sql
SELECT DATEDIFF('2026-09-30', '2026-09-22');
-- Output: 8

SELECT TIMESTAMPDIFF(MONTH, '2026-01-01', '2026-09-22');
-- Output: 8
```

* `DATEDIFF()` always returns the difference in days.
* `TIMESTAMPDIFF()` can return the difference in years, months, days, hours, minutes, or seconds.

### How do you calculate age in SQL?

```sql
SELECT TIMESTAMPDIFF(YEAR, '2000-01-01', CURDATE());
-- Output: Age in years
```

This is the most common interview use case for `TIMESTAMPDIFF()`.

### How do you find the last day of a month?

```sql
SELECT LAST_DAY('2026-09-22');
-- Output: 2026-09-30
```
Useful in payroll, billing, and reporting systems.