# String Functions

| Function        | Purpose                                                      | Common Syntax Variants                                  |
|-----------------|--------------------------------------------------------------|---------------------------------------------------------|
| `CONCAT()`      | Combines multiple strings into one string.                   | `CONCAT(str1, str2, ...)`                               |
| `CONCAT_WS()`   | Combines strings using a separator.                          | `CONCAT_WS(separator, str1, str2, ...)`                 |
| `LENGTH()`      | Returns length in bytes.                                     | `LENGTH(str)`                                           |
| `CHAR_LENGTH()` | Returns number of characters.                                | `CHAR_LENGTH(str)`                                      |
| `LOWER()`       | Converts string to lowercase.                                | `LOWER(str)`                                            |
| `UPPER()`       | Converts string to uppercase.                                | `UPPER(str)`                                            |
| `TRIM()`        | Removes leading and trailing spaces or specified characters. | `TRIM(str)`, `TRIM(chars FROM str)`                     |
| `LTRIM()`       | Removes leading spaces.                                      | `LTRIM(str)`                                            |
| `RTRIM()`       | Removes trailing spaces.                                     | `RTRIM(str)`                                            |
| `SUBSTRING()`   | Extracts part of a string.                                   | `SUBSTRING(str, pos)`, `SUBSTRING(str, pos, len)`       |
| `LEFT()`        | Returns leftmost characters.                                 | `LEFT(str, len)`                                        |
| `RIGHT()`       | Returns rightmost characters.                                | `RIGHT(str, len)`                                       |
| `REPLACE()`     | Replaces occurrences of a substring.                         | `REPLACE(str, old, new)`                                |
| `INSTR()`       | Returns position of first occurrence of substring.           | `INSTR(str, substr)`                                    |
| `LOCATE()`      | Finds position of substring.                                 | `LOCATE(substr, str)`, `LOCATE(substr, str, start_pos)` |
| `REVERSE()`     | Reverses a string.                                           | `REVERSE(str)`                                          |
| `LPAD()`        | Pads string on the left.                                     | `LPAD(str, len, pad_str)`                               |
| `RPAD()`        | Pads string on the right.                                    | `RPAD(str, len, pad_str)`                               |

## Examples

```sql
-- CONCAT
SELECT CONCAT('John', ' ', 'Doe');
-- Output: John Doe

-- CONCAT_WS
SELECT CONCAT_WS('-', '2026', '09', '22');
-- Output: 2026-09-22

-- LENGTH
SELECT LENGTH('Hello');
-- Output: 5

-- CHAR_LENGTH
SELECT CHAR_LENGTH('Hello');
-- Output: 5

-- LOWER
SELECT LOWER('HELLO');
-- Output: hello

-- UPPER
SELECT UPPER('hello');
-- Output: HELLO

-- TRIM
SELECT TRIM('  hello  ');
-- Output: hello

SELECT TRIM('x' FROM 'xxxhelloxxx');
-- Output: hello

-- LTRIM
SELECT LTRIM('   hello');
-- Output: hello

-- RTRIM
SELECT RTRIM('hello   ');
-- Output: hello

-- SUBSTRING
SELECT SUBSTRING('Database', 1, 4);
-- Output: Data

SELECT SUBSTRING('Database', 5);
-- Output: base

-- LEFT
SELECT LEFT('Database', 4);
-- Output: Data

-- RIGHT
SELECT RIGHT('Database', 4);
-- Output: base

-- REPLACE
SELECT REPLACE('Java Developer', 'Java', 'Backend');
-- Output: Backend Developer

-- INSTR
SELECT INSTR('Database', 'base');
-- Output: 5

-- LOCATE
SELECT LOCATE('base', 'Database');
-- Output: 5

SELECT LOCATE('a', 'Database', 3);
-- Output: 4

-- REVERSE
SELECT REVERSE('Database');
-- Output: esabataD

-- LPAD
SELECT LPAD('123', 5, '0');
-- Output: 00123

-- RPAD
SELECT RPAD('123', 5, '0');
-- Output: 12300
```

## Reference
[MYSQL Reference](https://dev.mysql.com/doc/refman/8.4/en/string-functions.html)