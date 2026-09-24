# Transactions (TCL)

## What is a Transaction?

A **transaction** is a group of one or more SQL statements that are executed as a single unit of work.

A transaction ensures that either:

```text
All operations succeed
OR
None of them are applied
```

This prevents the database from ending up in an inconsistent state.


## Why Do We Need Transactions?

Consider a bank transfer:

```text
Account A -> Account B

Step 1: Deduct ₹1000 from Account A
Step 2: Add ₹1000 to Account B
```

Suppose Step 1 succeeds but Step 2 fails.

Result:

```text
Account A lost ₹1000
Account B did not receive ₹1000
```

Data becomes inconsistent.

Transactions solve this problem by ensuring both operations succeed together or fail together.

## TCL Commands

TCL stands for **Transaction Control Language**.

| Command                 | Purpose                                   |
|-------------------------|-------------------------------------------|
| `START TRANSACTION`     | Begins a transaction                      |
| `COMMIT`                | Permanently saves changes                 |
| `ROLLBACK`              | Undoes changes since transaction start    |
| `SAVEPOINT`             | Creates a checkpoint inside a transaction |
| `ROLLBACK TO SAVEPOINT` | Rolls back to a checkpoint                |


### Commit
```sql
-- Begins a new transaction.
START TRANSACTION; or BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE account_id = 2;

-- Permanently saves all changes.
COMMIT;
```

### Rollback
```sql
START TRANSACTION;

UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 1;

ROLLBACK;
```

### SAVEPOINT

Creates a checkpoint within a transaction.

```sql
START TRANSACTION;

UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 1;

SAVEPOINT sp1;

UPDATE accounts
SET balance = balance + 1000
WHERE account_id = 2;
```
Now the transaction has a checkpoint called `sp1`.


### ROLLBACK TO SAVEPOINT
Rolls back only part of a transaction.

```sql
START TRANSACTION;

UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 1;

SAVEPOINT sp1;

UPDATE accounts
SET balance = balance + 1000
WHERE account_id = 2;

ROLLBACK TO sp1;

COMMIT;
```

Result:

```text
First update remains.
Second update is undone.
```

## Auto Commit

MySQL enables auto-commit by default. To check the status of `autocommit` we can use the following query:

```sql
SELECT @@autocommit; 
-- Outputs 1 or 0
-- 1 - Every statement is automatically committed.
```

To Disable Auto Commit: `SET autocommit = 0;`. Now changes remain pending until `COMMIT` or `ROLLBACK`.

## Transaction Example

```sql
-- Start transaction
START TRANSACTION;

-- Deduct money
UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 1;

-- Add money
UPDATE accounts
SET balance = balance + 1000
WHERE account_id = 2;

-- Save permanently
COMMIT;
```

Output:

```text
Transfer completed successfully.
```

