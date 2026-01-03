---
title: Data Manipulation Language (DML)
summary: Comprehensive guide to SQL Data Manipulation Language — SELECT, INSERT, UPDATE, DELETE, MERGE, transactions, performance, and best practices.
---

# Data Manipulation Language (DML)

Data Manipulation Language (DML) in SQL is the set of commands used to retrieve, insert, update, and delete data stored in relational databases. This chapter covers core DML statements, advanced usage patterns, transaction control, performance considerations, and common pitfalls.

## Table of Contents

- [Overview](#overview)
- [SELECT — Querying Data](#select---querying-data)
  - [Basic SELECT](#basic-select)
  - [Filtering: WHERE](#filtering-where)
  - [Projection: Choosing columns](#projection-choosing-columns)
  - [Sorting: ORDER BY](#sorting-order-by)
  - [Limiting & Paging: LIMIT / OFFSET](#limiting--paging-limit--offset)
  - [Aggregate Functions: GROUP BY, HAVING](#aggregate-functions-group-by-having)
  - [Joins: INNER, LEFT, RIGHT, FULL, CROSS](#joins-inner-left-right-full-cross)
  - [Subqueries & CTEs](#subqueries--ctes)
  - [Window Functions](#window-functions)
- [INSERT — Adding Data](#insert---adding-data)
  - [Single-row INSERT](#single-row-insert)
  - [Multi-row INSERT](#multi-row-insert)
  - [INSERT ... SELECT](#insert--select)
  - [Upsert / INSERT ... ON CONFLICT / REPLACE / MERGE](#upsert--insert--on-conflict--replace--merge)
- [UPDATE — Modifying Data](#update---modifying-data)
  - [Basic UPDATE](#basic-update)
  - [UPDATE with JOINs](#update-with-joins)
  - [UPDATE with CTEs](#update-with-ctes)
  - [Performance & Safety Tips](#performance--safety-tips)
- [DELETE — Removing Data](#delete---removing-data)
  - [DELETE with WHERE](#delete-with-where)
  - [DELETE with JOINs / Subqueries](#delete-with-joins--subqueries)
  - [TRUNCATE vs DELETE](#truncate-vs-delete)
- [MERGE — Conditional INSERT/UPDATE (SQL:2003)](#merge---conditional-insertupdate-sql2003)
- [Transactions & Concurrency](#transactions--concurrency)
  - [BEGIN / COMMIT / ROLLBACK](#begin--commit--rollback)
  - [Isolation Levels](#isolation-levels)
  - [Locking Considerations](#locking-considerations)
- [Performance Considerations](#performance-considerations)
  - [Indexes and DML](#indexes-and-dml)
  - [Bulk operations & batching](#bulk-operations--batching)
  - [Explain / Execution Plans](#explain--execution-plans)
- [Security & Best Practices](#security--best-practices)
- [Common Pitfalls](#common-pitfalls)
- [Examples & Exercises](#examples--exercises)
- [References & Further Reading](#references--further-reading)

---

## Overview

DML is concerned with the data inside tables. The most common operations are:

- `SELECT` — Read/query data
- `INSERT` — Add new rows
- `UPDATE` — Modify existing rows
- `DELETE` — Remove rows
- `MERGE` — Conditional insert or update (SQL standard; syntax varies)

Unlike Data Definition Language (DDL) commands (CREATE, ALTER, DROP), DML modifies the data and is typically covered by transaction semantics.

---

## SELECT — Querying Data

### Basic SELECT

```sql
SELECT column1, column2
FROM table_name;
```

Select all columns:

```sql
SELECT *
FROM employees;
```

Avoid `SELECT *` in production queries — it fetches unnecessary columns and can hurt performance.

### Filtering: WHERE

```sql
SELECT id, name, salary
FROM employees
WHERE department = 'sales' AND salary > 50000;
```

Operators: `=`, `!=`/`<>`, `<`, `>`, `<=`, `>=`, `BETWEEN`, `IN`, `LIKE`, `IS NULL`.

### Projection: Choosing columns

Select only necessary columns to reduce I/O and network overhead.

### Sorting: ORDER BY

```sql
SELECT id, name, salary
FROM employees
ORDER BY salary DESC, name ASC;
```

### Limiting & Paging: LIMIT / OFFSET

Postgres/MySQL:

```sql
SELECT * FROM employees
ORDER BY id
LIMIT 10 OFFSET 20; -- page 3 if page size is 10
```

SQL Server uses `OFFSET ... FETCH` or `TOP` for similar behavior.

### Aggregate Functions: GROUP BY, HAVING

```sql
SELECT department, COUNT(*) AS cnt, AVG(salary) AS avg_sal
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

`HAVING` is a filter applied after grouping — use it to filter aggregates.

### Joins: INNER, LEFT, RIGHT, FULL, CROSS

Inner join:

```sql
SELECT e.name, d.name AS dept
FROM employees e
JOIN departments d ON e.dept_id = d.id;
```

Left (outer) join:

```sql
SELECT e.name, d.name AS dept
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;
```

Right/Full joins behave similarly — full outer joins are not supported in all RDBMS.

Cross join (cartesian product):

```sql
SELECT * FROM colors CROSS JOIN sizes;
```

### Subqueries & CTEs

Subquery example:

```sql
SELECT name FROM employees
WHERE dept_id = (SELECT id FROM departments WHERE name = 'Sales');
```

CTE (Common Table Expression):

```sql
WITH top_sales AS (
  SELECT * FROM orders WHERE amount > 10000
)
SELECT * FROM top_sales WHERE created_at > '2024-01-01';
```

CTEs improve readability and can be recursive.

### Window Functions

Window functions provide row-wise aggregates without collapsing rows:

```sql
SELECT id, name, salary,
       RANK() OVER (PARTITION BY department ORDER BY salary DESC) as dept_rank
FROM employees;
```

---

## INSERT — Adding Data

### Single-row INSERT

```sql
INSERT INTO employees (name, dept_id, salary)
VALUES ('Alice', 3, 70000);
```

### Multi-row INSERT

```sql
INSERT INTO employees (name, dept_id, salary) VALUES
('Bob', 2, 50000),
('Carol', 1, 60000);
```

### INSERT ... SELECT

Copy rows between tables:

```sql
INSERT INTO employees_archive (id, name, dept_id, salary)
SELECT id, name, dept_id, salary
FROM employees
WHERE created_at < '2023-01-01';
```

### Upsert / INSERT ... ON CONFLICT / REPLACE / MERGE

Postgres `ON CONFLICT`:

```sql
INSERT INTO products (id, name, qty)
VALUES (1, 'widget', 10)
ON CONFLICT (id) DO UPDATE SET qty = products.qty + EXCLUDED.qty;
```

MySQL `ON DUPLICATE KEY UPDATE`:

```sql
INSERT INTO products (id, name, qty)
VALUES (1, 'widget', 10)
ON DUPLICATE KEY UPDATE qty = qty + VALUES(qty);
```

SQL Server / standard `MERGE`:

```sql
MERGE INTO products AS tgt
USING (VALUES (1, 'widget', 10)) AS src(id, name, qty)
ON tgt.id = src.id
WHEN MATCHED THEN UPDATE SET qty = tgt.qty + src.qty
WHEN NOT MATCHED THEN INSERT (id, name, qty) VALUES (src.id, src.name, src.qty);
```

Use upserts carefully — consider concurrency and race conditions.

---

## UPDATE — Modifying Data

### Basic UPDATE

```sql
UPDATE employees
SET salary = salary * 1.05
WHERE performance_rating = 'A';
```

**Warning**: Without a `WHERE` clause, `UPDATE` modifies every row.

### UPDATE with JOINs

Postgres syntax using `FROM`:

```sql
UPDATE employees e
SET dept_name = d.name
FROM departments d
WHERE e.dept_id = d.id;
```

MySQL supports multi-table `UPDATE` with `JOIN`:

```sql
UPDATE employees e
JOIN departments d ON e.dept_id = d.id
SET e.dept_name = d.name;
```

### UPDATE with CTEs

You can compute complex sets with CTEs and update them:

```sql
WITH mgr_salaries AS (
  SELECT id, salary FROM employees WHERE is_manager = true
)
UPDATE employees e
SET salary = salary * 1.10
FROM mgr_salaries m
WHERE e.id = m.id;
```

### Performance & Safety Tips

- Always run a `SELECT` with the same `WHERE` before executing `UPDATE`.
- Use transactions for multi-step updates.
- Consider row-limiting updates for very large tables (batching).

---

## DELETE — Removing Data

### DELETE with WHERE

```sql
DELETE FROM employees WHERE created_at < '2019-01-01';
```

### DELETE with JOINs / Subqueries

```sql
DELETE FROM employees e
USING departments d
WHERE e.dept_id = d.id AND d.name = 'Obsolete';
```

Or with subquery:

```sql
DELETE FROM employees WHERE dept_id IN (SELECT id FROM departments WHERE is_active = false);
```

### TRUNCATE vs DELETE

- `DELETE` removes rows and can be rolled back in a transaction (depending on DB). It activates triggers.
- `TRUNCATE` is faster, resets storage, and usually cannot be rolled back in some systems; it bypasses triggers.

Use `TRUNCATE` when you need an efficient table wipe and don't need per-row triggers or transactional rollback.

---

## MERGE — Conditional INSERT/UPDATE (SQL:2003)

`MERGE` combines `INSERT` and `UPDATE` logic based on matching keys. It's handy for data warehousing and ETL tasks. Syntax varies by vendor.

Basic pattern:

```sql
MERGE INTO target AS t
USING source AS s
ON (t.key = s.key)
WHEN MATCHED THEN UPDATE SET ...
WHEN NOT MATCHED THEN INSERT (...)
;
```

Be cautious: `MERGE` has had implementation bugs in some RDBMS historically; test carefully.

---

## Transactions & Concurrency

### BEGIN / COMMIT / ROLLBACK

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT; -- or ROLLBACK;
```

Transactions ensure atomicity: either all changes in the transaction commit or none do.

### Isolation Levels

Common levels:
- `READ UNCOMMITTED` — dirty reads allowed
- `READ COMMITTED` — avoid dirty reads
- `REPEATABLE READ` — stable reads within transaction
- `SERIALIZABLE` — highest isolation, may reduce concurrency

Choose appropriate level balancing correctness and performance.

### Locking Considerations

- Row-level locks are common; table locks may be taken for some operations.
- Long-running transactions increase lock contention and risk deadlocks.
- Use short transactions and batch updates to minimize locks.

---

## Performance Considerations

### Indexes and DML

- Indexes speed up `SELECT` but slow `INSERT`/`UPDATE`/`DELETE` because indexes must be maintained.
- For heavy write workloads, consider minimizing indexes or using bulk-loading techniques.

### Bulk operations & batching

- Insert/update in batches (e.g., 1,000 rows at a time) to reduce transaction overhead.
- Use native bulk loaders (e.g., `COPY` in Postgres, `LOAD DATA INFILE` in MySQL).

### Explain / Execution Plans

- Use `EXPLAIN`/`EXPLAIN ANALYZE` to investigate query performance.
- Check for sequential scans, index usage, join algorithms, and estimated vs actual rows.

---

## Security & Best Practices

- Use parameterized queries / prepared statements to prevent SQL injection.
- Principle of least privilege for DB users.
- Avoid constructing SQL with string concatenation of user input.
- Sanitize and validate inputs; however, do not rely on input validation alone for security.

---

## Common Pitfalls

- Running `UPDATE`/`DELETE` without `WHERE`.
- Using `SELECT *` in production queries.
- Neglecting transaction boundaries.
- Over-indexing write-heavy tables.
- Misusing `MERGE` without understanding concurrency semantics.

---

## Examples & Exercises

### Example 1: Find top 3 earning employees per department (Postgres `DISTINCT ON` vs Window)

```sql
SELECT DISTINCT ON (department) department, id, name, salary
FROM employees
ORDER BY department, salary DESC
LIMIT 3; -- not correct; use window functions instead

-- Correct using window functions
SELECT department, id, name, salary
FROM (
  SELECT department, id, name, salary,
         ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) as rn
  FROM employees
) t
WHERE rn <= 3;
```

### Exercise 1:

Write an `UPDATE` statement to increase salaries by 10% for employees with `performance_rating = 'A'` but only for departments where average salary is below 70000.

### Exercise 2:

Using `MERGE` or `ON CONFLICT`, write a statement to update product stock when shipments arrive; insert new product row if not present.

---

## References & Further Reading

- PostgreSQL Documentation — DML: https://www.postgresql.org/docs/current/dml.html
- MySQL Documentation — DML: https://dev.mysql.com/doc/refman/en/data-manipulation.html
- SQL Standard (ISO/IEC 9075) — MERGE
- Use `EXPLAIN` and `EXPLAIN ANALYZE` for query tuning

---

*Last updated: October 2025*
