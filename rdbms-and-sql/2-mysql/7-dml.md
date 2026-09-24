# Data Manipulation Language (DML)

DML (Data Manipulation Language) is a category of SQL commands used to insert, update, and delete data stored in database tables.

DML commands affect the data inside tables rather than the database structure.

### Commands

| Command  | Purpose                       |
|----------|-------------------------------|
| `INSERT` | Inserts new rows into a table |
| `UPDATE` | Modifies existing rows        |
| `DELETE` | Removes rows from a table     |


## 1. `INSERT`

### Syntax Variants (SQL)

```sql
-- Insert a Single Row
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);

-- Insert Multiple Rows
INSERT INTO table_name (column1, column2, ...)
VALUES
    (value1, value2, ...),
    (value1, value2, ...);

-- Insert Using SELECT
INSERT INTO target_table (column1, column2, ...)
SELECT column1, column2, ...
FROM source_table
WHERE condition;
```

### MySQL Specific Syntax Variants

```sql
-- Ignore Duplicate Key Errors
INSERT IGNORE INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);

-- Insert or Update (Upsert)
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...)
ON DUPLICATE KEY UPDATE
    column1 = value1,
    column2 = value2;
```

## 2. `UPDATE`

### Syntax Variants (SQL)

```sql
-- Update Specific Rows
UPDATE table_name
SET column1 = value1,
    column2 = value2
WHERE condition;

-- Update All Rows
UPDATE table_name
SET column1 = value1,
    column2 = value2;
```

### MySQL Specific Syntax Variants

```sql
-- Normal update is similar to that of SQL
-- Update Using JOIN
UPDATE table1 t1
JOIN table2 t2
    ON t1.column_name = t2.column_name
SET t1.column_name = value
WHERE condition;
```

## 3. `DELETE`

### Syntax Variants (SQL)

```sql
-- Delete Specific Rows
DELETE FROM table_name
WHERE condition;

-- Delete All Rows
DELETE FROM table_name;
```

### MySQL Specific Syntax Variants

```sql
-- Normal Delete is similar to that of SQL
-- Delete Using JOIN
DELETE t1
FROM table1 t1
JOIN table2 t2
    ON t1.column_name = t2.column_name
WHERE condition;
```

### Example
```sql
/* =========================================================
   DML (DATA MANIPULATION LANGUAGE) - COMPLETE EXAMPLE
   Theme: employee_projects
   ========================================================= */


/* =========================================================
   INSERT
   ========================================================= */

-- Insert a Single Row
INSERT INTO employee_projects (
    employee_id,
    first_name,
    last_name,
    email,
    age,
    salary,
    department_id,
    project_id
)
VALUES (
    1,
    'John',
    'Doe',
    'john@company.com',
    25,
    50000,
    1,
    101
);

-- Insert Multiple Rows
INSERT INTO employee_projects (
    employee_id,
    first_name,
    last_name,
    email,
    age,
    salary,
    department_id,
    project_id
)
VALUES
(
    2,
    'Alice',
    'Smith',
    'alice@company.com',
    28,
    60000,
    1,
    102
),
(
    3,
    'Bob',
    'Wilson',
    'bob@company.com',
    30,
    70000,
    2,
    103
);

-- Insert Using SELECT
INSERT INTO employee_archive (
    employee_id,
    first_name,
    last_name
)
SELECT
    employee_id,
    first_name,
    last_name
FROM employee_projects
WHERE salary > 60000;


/* =========================================================
   MYSQL SPECIFIC INSERT VARIANTS
   ========================================================= */

-- Ignore Duplicate Key Errors
INSERT IGNORE INTO employee_projects (
    employee_id,
    first_name,
    last_name,
    email
)
VALUES (
    1,
    'John',
    'Doe',
    'john@company.com'
);

-- Insert or Update Existing Row
INSERT INTO employee_projects (
    employee_id,
    first_name,
    last_name,
    email
)
VALUES (
    1,
    'John',
    'Doe',
    'john.updated@company.com'
)
ON DUPLICATE KEY UPDATE
    email = 'john.updated@company.com';


/* =========================================================
   UPDATE
   ========================================================= */

-- Update Specific Rows
UPDATE employee_projects
SET salary = 65000
WHERE employee_id = 1;

-- Update Multiple Columns
UPDATE employee_projects
SET
    salary = 70000,
    age = 26
WHERE employee_id = 1;

-- Update All Rows
UPDATE employee_projects
SET salary = salary + 5000;


/* =========================================================
   DELETE
   ========================================================= */

-- Delete Specific Rows
DELETE FROM employee_projects
WHERE employee_id = 1;

-- Delete All Rows
DELETE FROM employee_projects;
```