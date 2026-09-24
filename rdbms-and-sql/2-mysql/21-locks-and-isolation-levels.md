# Locks and Isolation Levels

## Why Do We Need Locks?

In a multi-user system, multiple transactions may try to read or modify the same data simultaneously.

Without proper control, this can lead to:

```text
Incorrect Data
Lost Updates
Dirty Reads
Inconsistent Results
```

Locks and isolation levels help ensure data consistency when multiple transactions run concurrently.

## What is a Lock?

A **lock** is a mechanism used by the database to control concurrent access to data.

When a transaction acquires a lock, other transactions may have to wait before accessing the same data.

## Row-Level vs Table-Level Locks

### Row-Level Lock

Locks only the affected rows.

Example:

```sql
UPDATE employees
SET salary = 70000
WHERE employee_id = 10;
```
Only employee 10 is locked.


MySQL InnoDB primarily uses row-level locking.

### Table-Level Lock

Locks the entire table.

Example:

```text
Employees table locked

No other transaction can modify rows
```

## Concurrency Problems

These problems occur when multiple transactions execute simultaneously.

### Dirty Read

A transaction reads data that has not yet been committed.

**Example**


```sql
-- Transaction T1
START TRANSACTION;

UPDATE accounts
SET balance = 5000
WHERE account_id = 1;

-- T1 not commited yet

-- Transaction T2
START TRANSACTION;
SELECT balance FROM accounts; -- T2 reads 5000

-- T1 rollbacks then actual value becomes 4000 but T2 read as 5000 which is invalid data.
-- This is called Dirty Read Problem

```

### Non-Repeatable Read

A row returns different values during the same transaction.

**Example**

```sql
-- T1
SELECT salary
FROM employees
WHERE employee_id = 1; -- Returns 50000

-- T2
UPDATE employees
SET salary = 60000
WHERE employee_id = 1;

COMMIT;

-- If T1 executes again the result is 60000
```
The same query returned different results. This is a **Non-Repeatable Read**.

### Phantom Read

New rows appear between repeated queries.

**Example**
```sql
-- T1
SELECT * FROM employees WHERE department_id = 10; -- Imagine outputs 5 rows

-- T2
INSERT INTO employees(...)
VALUES(...);

COMMIT;

-- If T1 runs the query again the output will be 6 rows
-- A new row appeared. This is called a Phantom Read.
```

## Isolation Levels
Isolation levels determine how much one transaction can see another transaction's changes.

### 1. READ UNCOMMITTED
Lowest isolation level available. It allows Dirty Read, Non-Repeatable Read, Phantom Read.

Maximum concurrency and minimum consistency.

### 2. READ COMMITTED
Only committed data can be read. This prevents Dirty Read problem. But still allows Non-Repeatable Read and Phantom Read

Used by many enterprise databases.

### 3. REPEATABLE READ
Default isolation level in MySQL InnoDB. This prevents Dirty Read,
Non-Repeatable Read but still allows Phantom Read 

(Although InnoDB uses MVCC and gap locking to reduce many phantom-read situations.)

### 4. SERIALIZABLE

Highest isolation level. This prevents Dirty Read,Non-Repeatable Read,Phantom Read.
Transactions execute as if they run one after another.

Highest consistency and Lowest concurrency.

## Setting Isolation Level

For current session:

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

## MVCC (Multi-Version Concurrency Control)

MySQL InnoDB uses MVCC to improve concurrency.

Instead of blocking readers and writers 
1. Readers see older committed versions.
2. Writers create new versions.

Benefits are Less Lock Contention, Higher Throughput and Better Performance