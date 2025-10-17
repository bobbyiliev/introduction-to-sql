## SQL NULLs and related functions

This document explains SQL NULL semantics and the most useful functions/operators for handling NULL values across common SQL dialects (PostgreSQL, MySQL, SQL Server, Oracle). It includes clear examples, common pitfalls, performance notes, and practice exercises.

### What is NULL?

NULL represents the absence of a value — unknown or not applicable. It is not the same as an empty string `''`, zero `0`, or a boolean `FALSE`.

- NULL means "unknown"; comparisons with NULL using normal comparison operators return `NULL` (treated as false in WHERE clauses).
- SQL uses three-valued logic: `TRUE`, `FALSE`, and `UNKNOWN` (the latter arises from expressions involving NULL).

Example:

```sql
SELECT NULL = NULL AS equal, NULL IS NULL AS is_null;
-- equal => NULL (UNKNOWN), is_null => true
```

### Checking for NULL: `IS NULL` / `IS NOT NULL`

Use `IS NULL` and `IS NOT NULL` to test for NULL explicitly. Do not use `= NULL` or `<> NULL`.

```sql
SELECT * FROM users WHERE last_login IS NULL;       -- users who never logged in
SELECT * FROM users WHERE email IS NOT NULL;        -- users who provided email
```

### `COALESCE` — choose the first non-NULL value

`COALESCE(expr1, expr2, ..., exprN)` returns the first non-NULL expression from the argument list.

Use cases:
- Provide default values when columns may be NULL.
- Replace chained `CASE` expressions.

Examples:

```sql
SELECT COALESCE(phone_mobile, phone_home, phone_work, 'no-phone') AS contact
FROM contacts;

-- Using COALESCE in aggregation contexts
SELECT department_id, COALESCE(SUM(bonus), 0) AS total_bonus
FROM employees
GROUP BY department_id;
```

Notes:
- `COALESCE` stops evaluating once it finds a non-NULL argument. Arguments may have side-effects in some DBMS (e.g., volatile functions).

### `NULLIF` — convert an equality into NULL

`NULLIF(expr1, expr2)` returns `NULL` if `expr1 = expr2`, otherwise returns `expr1`.

Use cases:
- Convert sentinel values to NULL (e.g., `-1` or empty string indicating missing value).

Example:

```sql
SELECT NULLIF(salary, 0) AS salary_or_null FROM payroll;
-- If salary equals 0, returns NULL; otherwise returns salary.
```

### `IS DISTINCT FROM` / `IS NOT DISTINCT FROM` (null-safe equality)

Some SQL dialects (Postgres, newer SQLite, and others) support `IS DISTINCT FROM` and `IS NOT DISTINCT FROM`. These perform null-safe comparisons where NULL is treated as a comparable value.

```sql
-- null-safe equality: true when both are null or when they are equal
SELECT a, b FROM t WHERE a IS NOT DISTINCT FROM b;
```

Note: MySQL has the `<=>` operator for null-safe equality (e.g., `a <=> b`). SQL Server does not implement `IS (NOT) DISTINCT FROM` natively.

### Dialect differences and functions

- PostgreSQL: full support for `COALESCE`, `NULLIF`, `IS DISTINCT FROM`, `IS NOT DISTINCT FROM`.
- MySQL: supports `COALESCE`, `NULLIF`, and the null-safe equality operator `<=>` (e.g., `a <=> b`).
- SQL Server (T-SQL): supports `COALESCE`, `NULLIF`, and `ISNULL(expr, replacement)` (T-SQL specific). `ISNULL` returns the replacement value if the expression is NULL. Note: `ISNULL` is different from `COALESCE` in evaluation and result type behavior.
- Oracle: supports `COALESCE` and `NULLIF`, plus `NVL(expr, replacement)` (similar to `ISNULL`). `NVL` returns `replacement` when `expr` is NULL.

Comparison notes between `COALESCE` and dialect-specific helpers:

- `COALESCE` is SQL-standard and preferable for portability.
- `ISNULL` (SQL Server) and `NVL` (Oracle) are vendor-specific and sometimes slightly different in type coercion rules and evaluation order.

### Aggregates and NULLs

- Aggregation functions (`SUM`, `AVG`, `COUNT`, `MAX`, `MIN`) typically ignore NULL values (except `COUNT(*)` which counts rows).

Examples:

```sql
SELECT SUM(amount) FROM payments;        -- ignores NULL amounts
SELECT COUNT(amount) FROM payments;      -- counts only rows where amount is not NULL
SELECT COUNT(*) FROM payments;           -- counts all rows
```

If you want to treat NULL as zero in aggregation, combine `COALESCE` or `SUM(COALESCE(x,0))`.

### NULLs in ORDER BY and GROUP BY

- Ordering: database systems differ in whether NULLs sort first or last by default (Postgres places NULLs first for ASC earlier versions? — prefer `NULLS FIRST` / `NULLS LAST` explicitly).

```sql
SELECT id, score FROM leaderboard ORDER BY score DESC NULLS LAST;
```

- GROUP BY: GROUP BY groups NULLs together; `GROUP BY` treats NULLs as equal for grouping purposes.

### Handling NULLs in JOINs

- LEFT/RIGHT/INNER JOIN behavior is not changed by NULLs directly, but matching on columns that can be NULL requires null-safe comparisons or `IS NULL` checks.

Example problem: find pairs where a.value = b.value but treat NULL=NULL as a match.

Postgres solution using `IS NOT DISTINCT FROM`:

```sql
SELECT a.id, b.id
FROM a JOIN b ON a.value IS NOT DISTINCT FROM b.value;
```

MySQL solution using `<=>`:

```sql
SELECT a.id, b.id
FROM a JOIN b ON a.value <=> b.value;
```

SQL Server workaround using `CASE`:

```sql
SELECT a.id, b.id
FROM a JOIN b
  ON (a.value = b.value) OR (a.value IS NULL AND b.value IS NULL);
```

### Null propagation and expressions

- Arithmetic or string expressions where one operand is NULL yield NULL (e.g., `1 + NULL` → NULL).
- Use `COALESCE` or explicit `IS NULL` checks to provide defaults.

Example:

```sql
SELECT price * COALESCE(quantity, 0) AS total
FROM sales;
```

### Performance considerations

- Index usage: testing for `IS NULL` can use an index in many DBMSs, but complex expressions with `COALESCE` or function-wrapped columns may prevent index usage (depends on DBMS and expression). Use generated/functional indexes if needed.
- Avoid wrapping indexed columns in functions in WHERE/JOIN predicates if you want to keep index seeks.
- Statistics and histograms might treat NULLs specially; be aware of the distribution of NULLs when planning queries.

### Best practices

1. Prefer `IS NULL` / `IS NOT NULL` for explicit null checks.
2. Use `COALESCE` for portable defaulting; use `ISNULL` / `NVL` only when targeting specific DBs.
3. Avoid relying on `= NULL` or `<> NULL` (they don't behave as expected).
4. When comparing nullable columns for equality in JOINs, prefer null-safe comparisons (`IS NOT DISTINCT FROM` or `<=>`), or add explicit `IS NULL` checks.
5. Be explicit about NULL ordering with `NULLS FIRST` / `NULLS LAST` if sorting matters.

### Examples and recipes

1) Replace missing text with a placeholder:

```sql
SELECT id, COALESCE(notes, '[no notes]') AS notes
FROM tasks;
```

2) Safe division (avoid division by NULL or zero):

```sql
SELECT a, b,
  CASE
    WHEN COALESCE(b, 0) = 0 THEN NULL
    ELSE a / b
  END AS ratio
FROM t;
```

3) Deduplicate rows treating NULLs as equal using window function (Postgres example):

```sql
SELECT DISTINCT ON (col1, col2) *
FROM items
ORDER BY col1, col2, created_at DESC;
-- Note: DISTINCT/ GROUP BY treat NULLs as equal for grouping
```

4) Convert sentinel values to NULL then aggregate:

```sql
SELECT customer_id, SUM(COALESCE(NULLIF(points, -1), 0)) AS points_sum
FROM loyalty
GROUP BY customer_id;
```

### Common pitfalls & gotchas

- Using `=` or `<>` with NULL yields `UNKNOWN` (treated as false in WHERE): `WHERE x = NULL` won't match anything.
- `COALESCE` returns the first non-NULL value and uses the type precedence rules of the DBMS to determine the result type.
- Vendor functions (`ISNULL`, `NVL`) may have different type coercion rules or evaluation semantics.

### Exercises

1. Given a `products` table with columns `(id, price, discount)`, write a query that returns the effective price using `discount` when present, otherwise `price`. If both `price` and `discount` are NULL, return 0.

2. You have two tables `a(id, value)` and `b(id, value)`. Write a query that returns pairs of ids where the values are equal or both NULL.

3. Given a `sales` table `(id, amount, discount)` where `discount` is NULL when no discount applies, write a query to compute total revenue treating NULL discount as 0 and excluding rows where `amount` is NULL.

4. (Advanced) Explain why `COALESCE(f(), g())` might call only `f()` in some DBMS and how to ensure both are safe to call.

### References

- SQL standard: COALESCE, NULLIF
- PostgreSQL docs: NULL handling and `IS DISTINCT FROM`
- MySQL docs: `<=>` operator and NULL handling
- SQL Server docs: `ISNULL`, `NULL` semantics

---

If you'd like, I can also:
- add dialect-specific examples in separate sections for PostgreSQL, MySQL, SQL Server, and Oracle;
- add runnable sample datasets and a small test script (SQL) you can run against SQLite/Postgres to try these examples;
- create a short quiz with answers.
