## Terminologies
### 1. SQL Keywords
**Definition:** Reserved words in SQL that have a predefined meaning and are used to write SQL statements.

Examples:
`SELECT`, `FROM`, `WHERE`, `INSERT`, `UPDATE`, `DELETE`, `CREATE`, `JOIN`

### 2. SQL Clauses
**Definition:** A clause is a section of an SQL statement that performs a specific function such as filtering, grouping, sorting, or specifying tables.

Examples:
```sql
FROM Employees
WHERE salary > 50000
GROUP BY department
ORDER BY salary DESC
```
Here, `FROM`, `WHERE`, `GROUP BY`, and `ORDER BY` are clauses.

### 3. SQL Expressions
**Definition:** An expression is a combination of values, columns, operators, and functions that evaluates to a single value.

Examples:
```sql
salary * 12
age + 5
UPPER(name)
salary > 50000
```

All of the above produce a single result/value.

### 4. SQL Statements
**Definition:** A statement is a complete SQL instruction sent to the database for execution.

Examples:
```sql
SELECT * FROM Employees;

INSERT INTO Employees VALUES (1, 'John');

CREATE TABLE Employees (
    id INT,
    name VARCHAR(50)
);
```
Each of the above is a complete statement.

### 5. SQL Commands
**Definition:** A command is a category of SQL statements grouped by purpose.

Types:

| Command Type | 	Purpose                   | 	Examples              |
|--------------|----------------------------|------------------------|
| DDL	         | Define database structure  | CREATE, ALTER, DROP    |
| DML	         | Modify data	               | INSERT, UPDATE, DELETE |
| DQL	         | Query data	                | SELECT                 |
| DCL	         | Control permissions	       | GRANT, REVOKE          |
| TCL	         | Manage transactions	       | COMMIT, ROLLBACK       |