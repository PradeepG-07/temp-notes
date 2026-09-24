# DCL (Data Control Language)

## What is DCL?

**DCL (Data Control Language)** consists of commands used to control access and permissions on database objects.

DCL allows database administrators to specify:

```text
Who can access data
What operations they can perform
Which database objects they can access
```

The two primary DCL commands are:

```text
GRANT
REVOKE
```

---

# GRANT

The `GRANT` command is used to give privileges to users or roles.

## Syntax

```sql
GRANT privilege_list
ON object_name
TO user_name;
```

---

## Grant SELECT Permission

```sql
GRANT SELECT
ON employees
TO user1;
```

Meaning:

```text
user1 can read data from employees table.
```

---

## Grant Multiple Permissions

```sql
GRANT SELECT, INSERT, UPDATE
ON employees
TO user1;
```

Meaning:

```text
user1 can:
Read Data
Insert Data
Update Data
```

---

## Grant All Permissions

```sql
GRANT ALL PRIVILEGES
ON employees
TO user1;
```

Meaning:

```text
user1 gets all available permissions on employees.
```

---

## Grant Permissions on Entire Database

```sql
GRANT ALL PRIVILEGES
ON company_db.*
TO user1;
```

Meaning:

```text
All tables inside company_db are accessible.
```

---

# REVOKE

The `REVOKE` command removes previously granted privileges.

## Syntax

```sql
REVOKE privilege_list
ON object_name
FROM user_name;
```

---

## Revoke SELECT Permission

```sql
REVOKE SELECT
ON employees
FROM user1;
```

Meaning:

```text
user1 can no longer read employees table.
```

---

## Revoke Multiple Permissions

```sql
REVOKE INSERT, UPDATE
ON employees
FROM user1;
```

---

## Revoke All Permissions

```sql
REVOKE ALL PRIVILEGES
ON employees
FROM user1;
```

---

# Common Privileges

| Privilege        | Description               |
| ---------------- | ------------------------- |
| `SELECT`         | Read data                 |
| `INSERT`         | Add new rows              |
| `UPDATE`         | Modify existing rows      |
| `DELETE`         | Remove rows               |
| `CREATE`         | Create database objects   |
| `DROP`           | Delete database objects   |
| `ALTER`          | Modify table structure    |
| `INDEX`          | Create or remove indexes  |
| `ALL PRIVILEGES` | All available permissions |

---

# Viewing User Permissions

## Show Grants

```sql
SHOW GRANTS FOR 'user1'@'localhost';
```

Output:

```text
Displays all permissions assigned to the user.
```

---

# Roles (MySQL 8+)

Instead of assigning permissions to every user individually, permissions can be assigned to roles.

## Create Role

```sql
CREATE ROLE developer_role;
```

---

## Grant Privileges to Role

```sql
GRANT SELECT, INSERT, UPDATE
ON company_db.*
TO developer_role;
```

---

## Assign Role to User

```sql
GRANT developer_role
TO user1;
```

Now:

```text
user1 inherits all permissions from developer_role.
```

---

# DCL vs TCL

| DCL                  | TCL                               |
| -------------------- | --------------------------------- |
| Controls permissions | Controls transactions             |
| `GRANT`, `REVOKE`    | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |
| Security focused     | Consistency focused               |

---

# Most Important for Interviews

```sql
GRANT
REVOKE
SHOW GRANTS

SELECT Privilege
INSERT Privilege
UPDATE Privilege
DELETE Privilege

Roles
```

---

# Common Interview Questions

## What is DCL?

DCL is used to control access and permissions on database objects.

---

## Which commands belong to DCL?

```text
GRANT
REVOKE
```

---

## Difference Between GRANT and REVOKE?

| GRANT             | REVOKE              |
| ----------------- | ------------------- |
| Gives permissions | Removes permissions |
| Provides access   | Restricts access    |

---

## What is a Role?

A role is a collection of privileges that can be assigned to one or more users.

---

## Interview Definition

> DCL (Data Control Language) is a category of SQL commands used to manage user permissions and access control within a database. The primary DCL commands are `GRANT`, which assigns privileges, and `REVOKE`, which removes them.
