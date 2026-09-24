# Data Definition Language
DDL (Data Definition Language) is a category of SQL commands used to define, create, modify, and remove database structures such as databases, tables, views, indexes, and constraints.

DDL commands affect the schema (structure) of the database rather than the data stored inside it.

### Commands
| Command    | Purpose                                                            |
|------------|--------------------------------------------------------------------|
| `CREATE`   | Creates a new database object (table, database, view, index, etc.) |
| `ALTER`    | Modifies the structure of an existing object                       |
| `DROP`     | Permanently removes an object                                      |
| `TRUNCATE` | Removes all rows from a table while keeping its structure          |
| `RENAME`   | Changes the name of an existing object (database-specific support) |

### 1. `CREATE`
```sql
-- Create a Database
CREATE DATABASE database_name;
       
-- Create a table
CREATE TABLE table_name (
    column1 datatype [constraints],
    column2 datatype [constraints],
    ...
);

-- Create a view
CREATE VIEW view_name AS
SELECT columns
FROM table_name
WHERE condition;

-- Create an Index
CREATE INDEX index_name
    ON table_name (column1, column2, ...);

-- Create Unique Index
CREATE UNIQUE INDEX index_name
    ON table_name (column_name);
```

### 2. `ALTER`
```sql
-- Add Column
ALTER TABLE table_name
ADD COLUMN column_name datatype [constraints];

-- Modify Column Datatype (MySQL)
ALTER TABLE table_name
MODIFY COLUMN column_name new_datatype;

-- Rename Column
ALTER TABLE table_name
RENAME COLUMN old_column_name TO new_column_name;

-- Drop Column
ALTER TABLE table_name
DROP COLUMN column_name;
     
-- Add Constraint
ALTER TABLE table_name
ADD CONSTRAINT constraint_name
constraint_definition;

-- Drop Constraint
ALTER TABLE table_name
DROP CONSTRAINT constraint_name;
     
-- Mysql specific Drop Constraint
ALTER TABLE table_name
DROP CONSTRAINT_keyword constraint_name; -- see example below
     
-- Rename Table
ALTER TABLE table_name
RENAME TO new_table_name;
```

### 3. `DROP`
```sql
-- Drop Database
DROP DATABASE database_name;
     
-- Drop Table
DROP TABLE table_name;

-- Drop View
DROP VIEW view_name;

-- Drop Index (MySQL)
DROP INDEX index_name
    ON table_name;
```

### 4. `TRUNCATE`
```sql
-- Truncate Table
TRUNCATE TABLE table_name;
```
#### What Remains?
Table Structure      -> Remains
Columns              -> Remain
Indexes              -> Remain
Constraints          -> Remain
Auto Increment       -> Usually Reset (DB-specific)
Data                 -> Removed

### 5. RENAME
```sql
-- Rename Table (MySQL)
RENAME TABLE old_table_name
TO new_table_name;
```

### Example For MYSQL
```sql
/* =========================================================
   MYSQL DDL (DATA DEFINITION LANGUAGE) - COMPLETE REFERENCE
   ========================================================= */

CREATE DATABASE IF NOT EXISTS company_db;

USE company_db;


/* =========================================================
   CREATE TABLES
   ========================================================= */

CREATE TABLE IF NOT EXISTS departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);

CREATE TABLE IF NOT EXISTS projects (
    project_id INT PRIMARY KEY,
    project_name VARCHAR(100) NOT NULL
);

CREATE TABLE IF NOT EXISTS employee_projects (

    /* Auto Increment Column */
    employee_id INT AUTO_INCREMENT,

    /* Basic Columns */
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(255),
    age INT,
    salary DECIMAL(10,2) DEFAULT 30000.00,
    department_id INT,
    project_id INT,
    joining_date DATE DEFAULT (CURRENT_DATE),

    /* Column-Level Constraint */
    UNIQUE (email),

    /* Composite Unique Constraint */
    CONSTRAINT uq_employee_project
        UNIQUE (employee_id, project_id),

    /* Check Constraints */
    CONSTRAINT chk_age
        CHECK (age >= 18),

    CONSTRAINT chk_salary
        CHECK (salary > 0),

    /* Composite Primary Key */
    CONSTRAINT pk_employee_project
        PRIMARY KEY (employee_id, project_id),

    /* Foreign Key with Actions */
    CONSTRAINT fk_employee_department
        FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
        ON DELETE CASCADE
        ON UPDATE CASCADE,

    CONSTRAINT fk_employee_project
        FOREIGN KEY (project_id)
        REFERENCES projects(project_id)
        ON DELETE SET NULL
        ON UPDATE CASCADE
);


/* =========================================================
   CREATE VIEW
   ========================================================= */

CREATE VIEW active_employee_projects AS
SELECT
    ep.employee_id,
    ep.project_id,
    ep.salary,
    d.department_name,
    p.project_name
FROM employee_projects ep
JOIN departments d
    ON ep.department_id = d.department_id
JOIN projects p
    ON ep.project_id = p.project_id
WHERE ep.salary > 50000;


/* =========================================================
   CREATE INDEX
   ========================================================= */

CREATE INDEX idx_department
ON employee_projects(department_id);

CREATE INDEX idx_department_project
ON employee_projects(department_id, project_id);

CREATE UNIQUE INDEX idx_email
ON employee_projects(email);


/* =========================================================
   ALTER TABLE
   ========================================================= */

ALTER TABLE employee_projects
ADD COLUMN phone_number VARCHAR(20);

ALTER TABLE employee_projects
MODIFY COLUMN phone_number VARCHAR(30);

ALTER TABLE employee_projects
RENAME COLUMN phone_number TO contact_number;

ALTER TABLE employee_projects
ADD CONSTRAINT chk_contact
CHECK (LENGTH(contact_number) >= 10);

ALTER TABLE employee_projects
ADD CONSTRAINT uq_contact
UNIQUE (contact_number);

ALTER TABLE employee_projects
ADD CONSTRAINT fk_contact_department
FOREIGN KEY (department_id)
REFERENCES departments(department_id);

ALTER TABLE employee_projects
ALTER salary SET DEFAULT 50000;

ALTER TABLE employee_projects
ALTER salary DROP DEFAULT;

ALTER TABLE employee_projects
DROP FOREIGN KEY fk_contact_department;

ALTER TABLE employee_projects
DROP CHECK chk_contact;

ALTER TABLE employee_projects
DROP COLUMN contact_number;

ALTER TABLE employee_projects
RENAME TO employee_assignments;


/* =========================================================
   TRUNCATE
   ========================================================= */

TRUNCATE TABLE employee_assignments;


/* =========================================================
   RENAME TABLE
   ========================================================= */

RENAME TABLE employee_assignments
TO employee_projects;


/* =========================================================
   DROP
   ========================================================= */

DROP INDEX idx_department
ON employee_projects;

DROP INDEX idx_department_project
ON employee_projects;

DROP INDEX idx_email
ON employee_projects;

DROP VIEW IF EXISTS active_employee_projects;

DROP TABLE IF EXISTS employee_projects;

DROP TABLE IF EXISTS projects;

DROP TABLE IF EXISTS departments;

DROP DATABASE IF EXISTS company_db;
```