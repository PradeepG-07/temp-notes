# Triggers

## What is a Trigger?

A **Trigger** is a database object that automatically executes when a specified event occurs on a table.

Triggers are attached to tables and run automatically when data is inserted, updated, or deleted.

A trigger cannot be executed manually.
## Why Use Triggers?

```text
Audit Logging
Data Validation
Automatic Updates
Business Rule Enforcement
```

## Trigger Events

| Event    | Description                          |
|----------|--------------------------------------|
| `INSERT` | Trigger fires when a row is inserted |
| `UPDATE` | Trigger fires when a row is updated  |
| `DELETE` | Trigger fires when a row is deleted  |

## Trigger Timing

| Timing   | Description               |
|----------|---------------------------|
| `BEFORE` | Executes before the event |
| `AFTER`  | Executes after the event  |

## Types of Triggers

```text
BEFORE INSERT
AFTER INSERT

BEFORE UPDATE
AFTER UPDATE

BEFORE DELETE
AFTER DELETE
```

## Create Trigger Syntax

```sql
CREATE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE
ON table_name
FOR EACH ROW
BEGIN
    SQL statements;
END;
```

### Trigger Example

```sql
DELIMITER //

CREATE TRIGGER before_employee_insert
BEFORE INSERT  -- This part changes accordingly
ON employees
FOR EACH ROW
BEGIN
    SET NEW.salary = ABS(NEW.salary);
END //

DELIMITER ;
```

## Delete Trigger Syntax

```sql
DROP TRIGGER trigger_name;
```

## OLD and NEW Keywords
Triggers can access row values with `NEW` and `OLD` keywords.

1. NEW: Represents the new row value.
2. OLD: Represents the previous row value.

```sql
NEW.salary
NEW.employee_name
OLD.salary
OLD.employee_name
```

### Example

```sql
CREATE TRIGGER audit_salary_change
AFTER UPDATE
ON employees
FOR EACH ROW
INSERT INTO audit_log(
    old_salary,
    new_salary
)
VALUES(
    OLD.salary,
    NEW.salary
);
```

## Advantages

```text
Automatic Execution
Enforces Business Rules
Audit Logging
Data Validation
```

## Disadvantages

```text
Hidden Logic
Harder Debugging
Performance Overhead
```

