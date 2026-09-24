# Partitioning

## What is Partitioning?

**Partitioning** is the process of dividing a large table into smaller logical pieces called partitions.

To users and applications:

```text
Still One Table
```

Internally:

```text
Partition 1
Partition 2
Partition 3
...
```

The database can access only the required partitions instead of scanning the entire table.

---

## Why Use Partitioning?

Large tables may contain:

```text
Millions of Rows
Billions of Rows
```

Without partitioning:

```text
Scan Entire Table
```

With partitioning:

```text
Scan Relevant Partitions Only
```

---

# Types of Partitioning

## RANGE Partitioning

Partitions rows based on value ranges.

Example:

```sql
PARTITION BY RANGE (YEAR(order_date))
```

```text
2022 -> Partition 1
2023 -> Partition 2
2024 -> Partition 3
```

---

## LIST Partitioning

Partitions rows based on predefined values.

Example:

```sql
PARTITION BY LIST(region_id)
```

```text
North -> Partition 1
South -> Partition 2
East  -> Partition 3
```

---

## HASH Partitioning

Rows are distributed using a hash function.

Example:

```sql
PARTITION BY HASH(customer_id)
PARTITIONS 4;
```

```text
customer_id % 4
```

determines the partition.

---

## KEY Partitioning

Similar to HASH partitioning but uses MySQL's internal hashing algorithm.

```sql
PARTITION BY KEY(customer_id)
PARTITIONS 4;
```

---

# RANGE Partition Example

```sql
CREATE TABLE sales (
    sale_id INT,
    sale_date DATE
)
PARTITION BY RANGE(YEAR(sale_date))
(
    PARTITION p2022 VALUES LESS THAN (2023),
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025)
);
```

---

# Partition Pruning

One of the biggest benefits of partitioning.

Query:

```sql
SELECT *
FROM sales
WHERE sale_date = '2024-05-10';
```

Database:

```text
Reads Only p2024
```

instead of all partitions.

This optimization is called **Partition Pruning**.

---

# Advantages

```text
Faster Queries
Easier Maintenance
Faster Archiving
Better Large Table Management
```

---

# Disadvantages

```text
More Complex Design
Additional Administration
Not Useful for Small Tables
```

---

# Partitioning vs Indexing

| Partitioning            | Indexing                 |
| ----------------------- | ------------------------ |
| Splits table            | Creates search structure |
| Helps very large tables | Helps row lookup         |
| Reduces scanned data    | Reduces lookup cost      |
| Often used together     | Often used together      |

---

# Most Important for Interviews

```sql
RANGE Partitioning
HASH Partitioning
LIST Partitioning
KEY Partitioning

Partition Pruning
```

---

# Common Interview Questions

## Does Partitioning Create Multiple Tables?

```text
No

Logically it is still one table.
```

---

## Does Partitioning Replace Indexes?

```text
No

Partitioning and Indexing solve different problems.
```

---

## What is Partition Pruning?

The database reads only the partitions relevant to the query instead of scanning every partition.

---

## When Should Partitioning Be Used?

```text
Very Large Tables
Historical Data
Time-Series Data
Audit Logs
Sales Records
```

---

## Interview Definition

> Partitioning is a technique that divides a large table into smaller logical partitions while keeping it accessible as a single table. It improves performance and manageability by allowing the database to access only the relevant partitions during query execution.
