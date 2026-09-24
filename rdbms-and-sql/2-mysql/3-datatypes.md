## Data Types
Data types define the kind of values that can be stored in a column and determine how the data is stored, processed, and retrieved.

Each database management system may have its own specific set of data types with slight variations. 

Choosing the appropriate data type for each column is crucial for optimizing storage, ensuring data integrity, and improving query performance.

> In MySQL there are three main data types: String, Numeric, Date and Time.

### String Data Types
| Data Type                     | Description                                                                                                                                                                                                                                                                          |
|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `CHAR(size)`                  | A fixed-length string (can contain letters, numbers, and special characters). `size` specifies the column length in characters, from 0 to 255. Default `size` is 1.                                                                                                                  |
| `VARCHAR(size)`               | A variable-length string (can contain letters, numbers, and special characters). `size` specifies the maximum column length in characters, from 0 to 65,535.                                                                                                                         |
| `TEXT(size)`                  | Holds a string with a maximum length of 65,535 bytes.                                                                                                                                                                                                                                |
| `TINYTEXT`                    | A TEXT column with a maximum length of 255 characters.                                                                                                                                                                                                                               |
| `MEDIUMTEXT`                  | A TEXT column with a maximum length of 16,777,215 (2^24 - 1) characters.                                                                                                                                                                                                             |
| `LONGTEXT`                    | A TEXT column with a maximum length of 4,294,967,295 characters (4 GB).                                                                                                                                                                                                              |
| `ENUM(val1, val2, val3, ...)` | A string object that can have only one value, chosen from the list of possible values (`val1`, `val2`, `val3`, ...). Up to 65,535 values can be listed. If an invalid value is inserted, error is thrown in strict mode of MYSQL otherwise a blank value is inserted with a warning. |
| `SET(val1, val2, val3, ...)`  | A string object that can have zero or more values, chosen from the list of possible values (`val1`, `val2`, `val3`, ...). Up to 64 values can be listed.                                                                                                                             |
- Each of the `size` parameters define the number of characters that can be stored.
- The size is determined by the charset (ASCII, UTF-8, UTF-16, etc.). Only ASCII characters are guaranteed to fit in 1 byte.
- `'Hello'` takes 5 characters and 5 bytes with `utf8mb4`. `'😀😀😀😀😀'` takes 5 characters but 20 bytes(5 * 4) because of different charset.

| Data Type         | Description                                                                                                            |
|-------------------|------------------------------------------------------------------------------------------------------------------------|
| `BINARY(size)`    | Similar to `CHAR()`, but stores binary byte strings. `size` specifies the column length in bytes. Default `size` is 1. |
| `VARBINARY(size)` | Similar to `VARCHAR()`, but stores binary byte strings. `size` specifies the maximum column length in bytes.           |
| `BLOB(size)`      | A BLOB column with a maximum length of 65,535 bytes.                                                                   |
| `TINYBLOB`        | A BLOB column with a maximum length of 255 bytes.                                                                      |
| `MEDIUMBLOB`      | A BLOB column with a maximum length of 16,777,215 bytes.                                                               |
| `LONGBLOB`        | A BLOB column with a maximum length of 4,294,967,295 bytes (4 GB).                                                     |

**Note:**
Although VARCHAR(n) defines a maximum of n characters, MySQL also enforces a maximum row size of about 65,535 bytes, so the allowed value of n depends on the character set and the total storage used by all columns in the row.
Example:
```sql
CREATE TABLE Employee (
    name VARCHAR(16000)
) CHARACTER SET utf8mb4;
```
Potential Size
```text
name = 16,000 × 4
     = 64,000 bytes
64,000 < 65,535
```
Result: Table is created successfully.
```sql
CREATE TABLE Employee (
    name VARCHAR(16000),
    age INT
) CHARACTER SET utf8mb4;
```
Potential Size
```text
name = 16,000 × 4 = 64,000 bytes
age  = 4 bytes
----------------------
Total = 64,004 bytes
```
At first glance this looks okay, but MySQL rows also require:
- Length bytes for VARCHAR
- NULL bitmap
- Row metadata/internal overhead

So the actual required size becomes:
```text
64,000
+ 4 (INT)
+ row overhead
  ≈ exceeds 65,535 bytes
```
Result: MySQL rejects the table definition.

## Numeric Data Types
### Bit Type

| Data Type   | Description                                                                                                       |
|-------------|-------------------------------------------------------------------------------------------------------------------|
| `BIT(size)` | A bit-value type. `size` indicates the number of bits per value, from 1 to 64. The default value for `size` is 1. |

### Boolean Types

| Data Type | Description                                                                   |
|-----------|-------------------------------------------------------------------------------|
| `BOOL`    | A value of zero is considered false, and non-zero values are considered true. |
| `BOOLEAN` | A value of zero is considered false, and non-zero values are considered true. |

**Notes**

* `BOOLEAN` is a synonym for `BOOL`.
* In MySQL, both are typically implemented as `TINYINT(1)`.

### Integer Types

| Data Type   | Signed Range                                            | Unsigned Range                  | Storage |
|-------------|---------------------------------------------------------|---------------------------------|---------|
| `TINYINT`   | -128 to 127                                             | 0 to 255                        | 1 byte  |
| `SMALLINT`  | -32,768 to 32,767                                       | 0 to 65,535                     | 2 bytes |
| `MEDIUMINT` | -8,388,608 to 8,388,607                                 | 0 to 16,777,215                 | 3 bytes |
| `INT`       | -2,147,483,648 to 2,147,483,647                         | 0 to 4,294,967,295              | 4 bytes |
| `BIGINT`    | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 | 0 to 18,446,744,073,709,551,615 | 8 bytes |

**Notes**

* `UNSIGNED` removes negative values and doubles the positive range.
* The old `size` parameter (`INT(11)`, `BIGINT(20)`, etc.) represented display width and is deprecated in modern MySQL.
* `INTEGER` is a synonym for `INT`.

### Fixed-Point Types (Exact Precision)

| Data Type          | Description                                                                                                                                                    |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `DECIMAL(size, d)` | An exact fixed-point number. `size` is the total number of digits (maximum 65). `d` is the number of digits after the decimal point. Default: `DECIMAL(10,0)`. |

**Examples**

| Definition      | Valid Values       |
|-----------------|--------------------|
| `DECIMAL(5,2)`  | `123.45`, `999.99` |
| `DECIMAL(10,2)` | `12345678.99`      |

**Notes**

* Used for financial and monetary calculations.
* Stores values exactly without rounding errors.

### Floating-Point Types (Approximate Precision)

| Data Type | Description                                                                    | Storage |
|-----------|--------------------------------------------------------------------------------|---------|
| `FLOAT`   | Single-precision floating-point number. Approximate values.                    | 4 bytes |
| `DOUBLE`  | Double-precision floating-point number. More precision and range than `FLOAT`. | 8 bytes |

**Examples**

| Type     | Example             |
|----------|---------------------|
| `FLOAT`  | `3.1415926`         |
| `DOUBLE` | `3.141592653589793` |

**Notes**

* `FLOAT` and `DOUBLE` store approximate values and may introduce rounding errors.
* Avoid using them for money-related calculations.
* `FLOAT(p)`:

    * `p = 0–24` → behaves as `FLOAT`
    * `p = 25–53` → behaves as `DOUBLE`
* `DOUBLE PRECISION` is a synonym for `DOUBLE`.

### Common Interview Rule

| Requirement             | Recommended Type       |
| ----------------------- | ---------------------- |
| True/False value        | `BOOLEAN` / `BOOL`     |
| Small whole number      | `TINYINT` / `SMALLINT` |
| General whole number    | `INT`                  |
| Very large whole number | `BIGINT`               |
| Money, salary, prices   | `DECIMAL`              |
| Scientific calculations | `FLOAT` / `DOUBLE`     |
| Bit flags, permissions  | `BIT`                  |

## Date Types
### Date Type

| Data Type | Description                                                                              |
|-----------|------------------------------------------------------------------------------------------|
| `DATE`    | Stores only a date. Format: `YYYY-MM-DD`. Supported range: `1000-01-01` to `9999-12-31`. |

Example values are `2003-07-07`, `2003-09-09` etc.

### Date and Time Types

| Data Type        | Format                | Supported Range                                        |
|------------------|-----------------------|--------------------------------------------------------|
| `DATETIME(fsp)`  | `YYYY-MM-DD HH:MM:SS` | `1000-01-01 00:00:00` to `9999-12-31 23:59:59`         |
| `TIMESTAMP(fsp)` | `YYYY-MM-DD HH:MM:SS` | `1970-01-01 00:00:01 UTC` to `2038-01-19 03:14:07 UTC` |

### DATETIME
- Stores a date and time value directly.
- Not tied to a time zone.
- Suitable for historical and future dates.
- `fsp` (Fractional Seconds Precision) specifies the number of digits stored after the seconds part in TIME, DATETIME, and TIMESTAMP values. It ranges from 0 to 6, where 6 allows microsecond precision (e.g., 2026-09-18 10:30:45.123456).

**Example**

```sql
created_at DATETIME
```

Values:

```text
2026-09-18 10:30:45
2045-12-25 09:00:00
```

### TIMESTAMP
- Internally stored as seconds since the Unix Epoch (`1970-01-01 00:00:00 UTC`).
- Time-zone aware in MySQL (converted between session time zone and UTC).
- Commonly used for audit columns.

**Example**

```sql
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
```

Values:

```text
2026-09-18 10:30:45
```

**Automatic Initialization and Update**

```sql
updated_at TIMESTAMP
DEFAULT CURRENT_TIMESTAMP
ON UPDATE CURRENT_TIMESTAMP
```
Whenever the row is updated, `updated_at` automatically changes to the current timestamp.

### Time Type

| Data Type   | Description                                                                                 |
|-------------|---------------------------------------------------------------------------------------------|
| `TIME(fsp)` | Stores only a time value. Format: `HH:MM:SS`. Supported range: `-838:59:59` to `838:59:59`. |

Examples are `10:30:45`, `23:59:59`, `-10:15:00`, `150:45:20`

**Common Use Cases**
- Duration tracking
- Work hours
- Video/audio lengths
- Time intervals

**Note**
- `TIME` is not limited to 24 hours.
- It can represent durations longer than a day.

### Year Type

| Data Type | Description                                                                                |
|-----------|--------------------------------------------------------------------------------------------|
| `YEAR`    | Stores a year in four-digit format (`YYYY`). Allowed values: `1901` to `2155`, and `0000`. |

Examples are `2026`, `1999`, `2155` etc.

**Common Use Cases**
- Manufacturing year 
- Graduation year 
- Release year 
- Model year

### DATETIME vs TIMESTAMP (Interview Table)

| Feature              | DATETIME                                 | TIMESTAMP                                 |
|----------------------|------------------------------------------|-------------------------------------------|
| Storage              | 8 bytes                                  | 4 bytes                                   |
| Range                | 1000–9999                                | 1970–2038                                 |
| Time-zone conversion | No                                       | Yes                                       |
| Stores               | Actual date-time value                   | Seconds since Unix epoch                  |
| Best for             | Birth dates, appointments, future events | Audit fields (`created_at`, `updated_at`) |

---

### Common Interview Rule

| Requirement                | Recommended Type |
|----------------------------|------------------|
| Only date                  | `DATE`           |
| Only year                  | `YEAR`           |
| Only time/duration         | `TIME`           |
| General date-time          | `DATETIME`       |
| Created/updated timestamps | `TIMESTAMP`      |


## Others
### JSON
| Data Type | Description                                                              |
|-----------|--------------------------------------------------------------------------|
| `JSON`    | Stores JSON documents. MySQL validates that inserted data is valid JSON. |

### GEOSPATIAL
| Data Type         | Description                           |
|-------------------|---------------------------------------|
| `POINT`           | Single location (latitude, longitude) |
| `LINESTRING`      | Collection of connected points        |
| `POLYGON`         | Closed area                           |
| `GEOMETRY`        | Generic spatial object                |
| `MULTIPOINT`      | Multiple points                       |
| `MULTILINESTRING` | Multiple lines                        |
| `MULTIPOLYGON`    | Multiple polygons                     |


> Todo: Add an example of how to create table with this data type and how to insert a value.