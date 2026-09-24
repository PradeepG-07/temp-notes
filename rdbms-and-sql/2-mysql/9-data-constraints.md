# Data Constraints

## What are Constraints?

Constraints are rules enforced on table columns to ensure the accuracy, consistency, and integrity of data stored in a database.

Constraints help:

* Prevent invalid data from being inserted.
* Maintain relationships between tables.
* Enforce business rules.
* Improve data quality and reliability.

# Types of Constraints

MySQL provides the following commonly used constraints:

| Constraint    | Purpose                                         |
|---------------|-------------------------------------------------|
| `NOT NULL`    | Prevents NULL values.                           |
| `UNIQUE`      | Ensures all values are unique.                  |
| `PRIMARY KEY` | Uniquely identifies each row.                   |
| `FOREIGN KEY` | Maintains referential integrity between tables. |
| `CHECK`       | Ensures values satisfy a condition.             |
| `DEFAULT`     | Provides a default value when none is supplied. |

### NOT NULL
Ensures that a column cannot contain NULL values.

```sql
name VARCHAR(100) NOT NULL
```

### UNIQUE
Ensures that all values in a column are unique.

```sql
email VARCHAR(255) UNIQUE
```

### PRIMARY KEY
Uniquely identifies each row in a table.

```sql
employee_id INT PRIMARY KEY
```

* Must contain unique values.
* Cannot contain NULL values.
* Only one primary key is allowed per table.
* A primary key can consist of multiple columns (composite primary key).

## FOREIGN KEY
A foreign key in SQL is a column or group of columns in one table that refers to the primary key of another table. It establishes a link between two tables, enforcing referential integrity and maintaining relationships between related data.

```sql
FOREIGN KEY (department_id)
REFERENCES departments(department_id)
```

* References a primary key or unique key in another table.
* Prevents invalid references.
* Maintains referential integrity.

## CHECK
Ensures that inserted values satisfy a specified condition.

```sql
salary DECIMAL(10,2) CHECK (salary > 0)
```

* Enforced in MySQL 8.0.16 and later.
* Useful for business rule validation.

## DEFAULT
Assigns a default value when none is provided.

```sql
status VARCHAR(20) DEFAULT 'ACTIVE'
```

# Ways to Add Constraints
Constraints can be added in two ways:

## 1. During Table Creation
Using the `CREATE TABLE` statement.

### Column-Level Constraint
Applied directly to a column.

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    salary DECIMAL(10,2) NOT NULL CHECK (salary > 0)
);
```

### Table-Level Constraint
Defined separately within the table definition.

```sql
CREATE TABLE employees (
    employee_id INT,
    department_id INT,
    
    CONSTRAINT pk_employee
       PRIMARY KEY (employee_id),
    
    CONSTRAINT fk_employee_department
       FOREIGN KEY (department_id)
           REFERENCES departments(department_id)
);
```

# 2. After Table Creation

## Add a Single Constraint

### PRIMARY KEY

```sql
ALTER TABLE employees
ADD CONSTRAINT pk_employee
PRIMARY KEY (employee_id);
```

### UNIQUE

```sql
ALTER TABLE employees
ADD CONSTRAINT uq_employee_email
UNIQUE (email);
```

### FOREIGN KEY

```sql
ALTER TABLE employees
ADD CONSTRAINT fk_employee_department
FOREIGN KEY (department_id)
REFERENCES departments(department_id);
```

### CHECK

```sql
ALTER TABLE employees
ADD CONSTRAINT chk_salary
CHECK (salary > 0);
```

## Add Multiple Constraints

Multiple constraints can be added in a single `ALTER TABLE` statement.

```sql
ALTER TABLE employees
ADD CONSTRAINT uq_employee_email UNIQUE (email),
ADD CONSTRAINT chk_salary CHECK (salary > 0);
```

## Drop a Single Constraint

### Drop Primary Key

```sql
ALTER TABLE employees
DROP PRIMARY KEY;
```

### Drop Unique Constraint

```sql
ALTER TABLE employees
DROP INDEX uq_employee_email;
```

### Drop Foreign Key

```sql
ALTER TABLE employees
DROP FOREIGN KEY fk_employee_department;
```

### Drop Check Constraint

```sql
ALTER TABLE employees
DROP CHECK chk_salary;
```

## Drop Multiple Constraints
Multiple constraints can be removed in a single statement.

```sql
ALTER TABLE employees
DROP INDEX uq_employee_email,
DROP CHECK chk_salary;
```

## Modify a Constraint

MySQL generally does not support directly modifying most constraints.

The common approach is:

1. Drop the existing constraint.
2. Create the new constraint.

Example:

```sql
ALTER TABLE employees
DROP CHECK chk_salary;

ALTER TABLE employees
ADD CONSTRAINT chk_salary
CHECK (salary >= 1000);
```

# Constraint Naming

Constraints may have system generated names or user defined names.

## System-Generated Names

Generated automatically by MySQL.

```sql
email VARCHAR(255) UNIQUE
```

## User-Defined Names

Specified explicitly using the `CONSTRAINT` keyword.

```sql
CONSTRAINT uq_employee_email
UNIQUE (email)
```

### Benefits

* Easier maintenance.
* Easier debugging.
* Simpler constraint removal.


## Interview Notes

### PRIMARY KEY vs UNIQUE

| Feature         | PRIMARY KEY | UNIQUE   |
|-----------------|-------------|----------|
| Uniqueness      | Yes         | Yes      |
| NULL Allowed    | No          | Yes      |
| Count per Table | One         | Multiple |

### NOT NULL vs CHECK

| Feature             | NOT NULL | CHECK |
|---------------------|----------|-------|
| Prevents NULL       | Yes      | Can   |
| Supports Conditions | No       | Yes   |

### Can a table have multiple PRIMARY KEY constraints?
No. A table can have only one primary key. However, that primary key may consist of multiple columns.

```sql
PRIMARY KEY (employee_id, department_id)
```

### Can a FOREIGN KEY reference a UNIQUE key?
Yes. A foreign key can reference either:
* A PRIMARY KEY
* A UNIQUE KEY

### How many NULL values are allowed in a UNIQUE column in MySQL?
MySQL allows multiple NULL values in a UNIQUE column because NULL is treated as an unknown value.

```sql
email VARCHAR(255) UNIQUE
```

Multiple rows can contain NULL in the `email` column.
