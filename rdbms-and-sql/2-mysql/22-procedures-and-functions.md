# Stored Procedures and Functions

## What are Stored Procedures?

A **Stored Procedure** is a named collection of SQL statements stored inside the database that can be executed whenever needed.

Instead of sending multiple SQL statements from the application, you can store the logic in the database and call it using its name.

### Why Use Stored Procedures?

```text
Code Reusability
Reduced Network Traffic
Centralized Business Logic
Better Maintainability
```

## Stored Procedure Syntax

### Create Procedure

```sql
DELIMITER //

CREATE PROCEDURE procedure_name()
BEGIN
    SQL statements;
END //

DELIMITER ;
```

Example
```sql
-- Create a procedure
DELIMITER //

CREATE PROCEDURE GetAllEmployees()
BEGIN
    SELECT *
    FROM employees;
END //

DELIMITER ;
    
    
-- Execute Procedure
CALL GetAllEmployees();
```

### Procedure with Input Parameters

```sql
-- Create a procedure
DELIMITER //

CREATE PROCEDURE GetEmployeeById(
    IN emp_id INT
)
BEGIN
    SELECT *
    FROM employees
    WHERE employee_id = emp_id;
END //

DELIMITER ;

-- Execute Procedure
CALL GetEmployeeById(101);
```

### Procedure with OUT Parameters

```sql
-- Create a procedure
DELIMITER //

CREATE PROCEDURE GetEmployeeCount(
    OUT total_count INT
)
BEGIN
    SELECT COUNT(*)
    INTO total_count
    FROM employees;
END //

DELIMITER ;
    
-- Execute Procedure
CALL GetEmployeeCount(@count);
SELECT @count;
```

### Procedure with INOUT Parameters

```sql
-- Create a procedure
DELIMITER //

CREATE PROCEDURE IncreaseValue(
    INOUT value INT
)
BEGIN
    SET value = value + 100;
END //

DELIMITER ;
    
-- Execute Procedure
SET @num = 500;

CALL IncreaseValue(@num);

SELECT @num;
```

### Delete Procedure

```sql
DROP PROCEDURE procedure_name;
```

> **Delimiter** is a client-side statement terminator that tells MySQL tools where a statement ends; when creating stored procedures/functions, we temporarily change it from `;` to something like `//` so the client doesn't mistakenly treat the semicolons inside the procedure as the end of the statement and send an incomplete definition to the server.


## What are Functions?

A **Function** is a database object that accepts input values and returns a single value.

Functions are commonly used inside SQL queries.

### Function Syntax

```sql
DELIMITER //

CREATE FUNCTION function_name(...)
RETURNS datatype
DETERMINISTIC
BEGIN
    RETURN value;
END //

DELIMITER ;
```

`DETERMINISTIC` indicates that a stored function always returns the same result for the same input values, while `NOT DETERMINISTIC` means the result may vary even when the inputs remain unchanged.

### Example

```sql
DELIMITER //

CREATE FUNCTION GetBonus(
    salary DECIMAL(10,2)
)
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN salary * 0.10;
END //

DELIMITER ;

-- Execute Function
SELECT GetBonus(50000); -- Output: 5000

-- Function Used Inside Query
SELECT employee_name,
       salary,
       GetBonus(salary)
FROM employees;
```

## Procedure vs Function

| Feature                    | Procedure      | Function     |
|----------------------------|----------------|--------------|
| Returns Value              | Optional       | Mandatory    |
| Can Return Multiple Values | Yes            | No           |
| Called Using               | `CALL`         | `SELECT`     |
| Can Be Used Inside Queries | No             | Yes          |
| Purpose                    | Business Logic | Calculations |

### Can a Function Modify Tables?

Generally not recommended.

Functions are primarily used for calculations and value generation.