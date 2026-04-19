# SQL Basics Guide

A practical introduction to querying and managing relational databases with SQL.

---

## Table of Contents

1. [What is SQL?](#what-is-sql)
2. [Core Concepts](#core-concepts)
3. [SELECT — Querying Data](#select--querying-data)
4. [WHERE — Filtering Rows](#where--filtering-rows)
5. [ORDER BY — Sorting](#order-by--sorting)
6. [LIMIT and OFFSET](#limit-and-offset)
7. [Aggregate Functions](#aggregate-functions)
8. [GROUP BY and HAVING](#group-by-and-having)
9. [JOIN — Combining Tables](#join--combining-tables)
10. [INSERT — Adding Data](#insert--adding-data)
11. [UPDATE — Modifying Data](#update--modifying-data)
12. [DELETE — Removing Data](#delete--removing-data)
13. [CREATE TABLE](#create-table)
14. [ALTER and DROP](#alter-and-drop)
15. [Constraints](#constraints)
16. [Quick Reference Cheat Sheet](#quick-reference-cheat-sheet)

---

## What is SQL?

SQL (Structured Query Language) is the standard language for interacting with **relational databases** — systems that store data in structured tables with rows and columns.

**Common databases that use SQL:**
- PostgreSQL
- MySQL / MariaDB
- SQLite
- Microsoft SQL Server
- Oracle

Most SQL syntax is consistent across these, though each has minor differences.

---

## Core Concepts

| Term | Meaning |
|------|---------|
| **Table** | A collection of rows and columns, like a spreadsheet |
| **Row / Record** | A single entry in a table |
| **Column / Field** | A single attribute stored for every row |
| **Primary Key** | A column (or set of columns) that uniquely identifies each row |
| **Foreign Key** | A column that references the primary key of another table |
| **Query** | A request to read or modify data |
| **Schema** | The structure of the database — its tables and their columns |

### Example Tables Used in This Guide

**`users`**

| id | name    | email               | age | city      |
|----|---------|---------------------|-----|-----------|
| 1  | Alice   | alice@example.com   | 30  | London    |
| 2  | Bob     | bob@example.com     | 25  | Paris     |
| 3  | Charlie | charlie@example.com | 35  | London    |
| 4  | Diana   | diana@example.com   | 28  | New York  |

**`orders`**

| id | user_id | product   | amount | status    |
|----|---------|-----------|--------|-----------|
| 1  | 1       | Laptop    | 999.00 | shipped   |
| 2  | 1       | Mouse     | 29.99  | delivered |
| 3  | 2       | Keyboard  | 79.99  | pending   |
| 4  | 3       | Monitor   | 349.00 | shipped   |

---

## SELECT — Querying Data

```sql
-- All columns from a table
SELECT * FROM users;

-- Specific columns
SELECT name, email FROM users;

-- With an alias (rename a column in the output)
SELECT name AS full_name, email AS contact FROM users;

-- Distinct values (no duplicates)
SELECT DISTINCT city FROM users;

-- Computed columns
SELECT name, age, age + 1 AS next_birthday FROM users;
```

> `SELECT *` is fine for exploration, but in production code name your columns explicitly so queries don't break if the table changes.

---

## WHERE — Filtering Rows

```sql
SELECT * FROM users WHERE city = 'London';
SELECT * FROM users WHERE age > 28;
SELECT * FROM users WHERE age BETWEEN 25 AND 32;
SELECT * FROM users WHERE city IN ('London', 'Paris');
SELECT * FROM users WHERE city NOT IN ('New York');
SELECT * FROM users WHERE name LIKE 'A%';    -- Starts with A
SELECT * FROM users WHERE name LIKE '%e';    -- Ends with e
SELECT * FROM users WHERE name LIKE '%li%';  -- Contains "li"
SELECT * FROM users WHERE email IS NULL;
SELECT * FROM users WHERE email IS NOT NULL;
```

### Combining Conditions

```sql
SELECT * FROM users WHERE city = 'London' AND age > 28;
SELECT * FROM users WHERE city = 'London' OR city = 'Paris';
SELECT * FROM users WHERE NOT city = 'New York';

-- Group conditions with parentheses
SELECT * FROM users
WHERE (city = 'London' OR city = 'Paris') AND age >= 30;
```

### Comparison Operators

| Operator | Meaning |
|----------|---------|
| `=` | Equal |
| `!=` or `<>` | Not equal |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |
| `BETWEEN a AND b` | Inclusive range |
| `IN (...)` | Matches any value in the list |
| `LIKE` | Pattern match (`%` = any chars, `_` = one char) |
| `IS NULL` | Value is null |

---

## ORDER BY — Sorting

```sql
SELECT * FROM users ORDER BY age;             -- Ascending (default)
SELECT * FROM users ORDER BY age DESC;        -- Descending
SELECT * FROM users ORDER BY city, age DESC;  -- Multiple columns
```

---

## LIMIT and OFFSET

```sql
SELECT * FROM users LIMIT 10;            -- First 10 rows
SELECT * FROM users LIMIT 10 OFFSET 20; -- Rows 21–30 (for pagination)
```

> In SQL Server, use `TOP` instead: `SELECT TOP 10 * FROM users;`

---

## Aggregate Functions

Aggregate functions compute a single value from a set of rows.

```sql
SELECT COUNT(*) FROM users;                 -- Total rows
SELECT COUNT(email) FROM users;             -- Non-null emails only
SELECT SUM(amount) FROM orders;             -- Total order value
SELECT AVG(age) FROM users;                 -- Average age
SELECT MIN(age) FROM users;                 -- Youngest user
SELECT MAX(amount) FROM orders;             -- Highest order amount
```

---

## GROUP BY and HAVING

`GROUP BY` groups rows that share a value so you can aggregate each group.

```sql
-- Count users per city
SELECT city, COUNT(*) AS user_count
FROM users
GROUP BY city;

-- Total revenue per user
SELECT user_id, SUM(amount) AS total_spent
FROM orders
GROUP BY user_id;

-- Average age per city
SELECT city, AVG(age) AS avg_age
FROM users
GROUP BY city
ORDER BY avg_age DESC;
```

### HAVING

`WHERE` filters rows before grouping. `HAVING` filters groups after aggregation.

```sql
-- Only cities with more than 1 user
SELECT city, COUNT(*) AS user_count
FROM users
GROUP BY city
HAVING COUNT(*) > 1;

-- Users who have spent more than 500 total
SELECT user_id, SUM(amount) AS total_spent
FROM orders
GROUP BY user_id
HAVING SUM(amount) > 500;
```

---

## JOIN — Combining Tables

JOINs combine rows from two tables based on a related column.

### INNER JOIN

Returns only rows that have a match in both tables.

```sql
SELECT users.name, orders.product, orders.amount
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```

### LEFT JOIN

Returns all rows from the left table, with matching rows from the right (or NULL if no match).

```sql
SELECT users.name, orders.product
FROM users
LEFT JOIN orders ON users.id = orders.user_id;
-- Diana will appear with NULL for product (no orders)
```

### RIGHT JOIN

All rows from the right table, matched with the left (or NULL if no match).

```sql
SELECT users.name, orders.product
FROM users
RIGHT JOIN orders ON users.id = orders.user_id;
```

### Using Aliases to Shorten Queries

```sql
SELECT u.name, o.product, o.amount
FROM users u
JOIN orders o ON u.id = o.user_id
WHERE o.status = 'shipped';
```

### JOIN Types Summary

| Type | Returns |
|------|---------|
| `INNER JOIN` | Only matched rows from both tables |
| `LEFT JOIN` | All left rows + matched right rows (NULLs for unmatched) |
| `RIGHT JOIN` | All right rows + matched left rows (NULLs for unmatched) |
| `FULL OUTER JOIN` | All rows from both tables (NULLs for unmatched) |

---

## INSERT — Adding Data

```sql
-- Insert one row
INSERT INTO users (name, email, age, city)
VALUES ('Eve', 'eve@example.com', 22, 'Berlin');

-- Insert multiple rows at once
INSERT INTO users (name, email, age, city)
VALUES
  ('Frank', 'frank@example.com', 40, 'Tokyo'),
  ('Grace', 'grace@example.com', 31, 'Sydney');
```

> If a column has a default value or is auto-incremented (like `id`), you can omit it from the column list.

---

## UPDATE — Modifying Data

```sql
-- Update one column for a specific row
UPDATE users SET city = 'Manchester' WHERE id = 1;

-- Update multiple columns
UPDATE users SET city = 'Manchester', age = 31 WHERE id = 1;

-- Update all rows in a table (rarely intentional)
UPDATE orders SET status = 'archived';
```

> **Always use a `WHERE` clause with `UPDATE`.** Without it, every row in the table is modified.

---

## DELETE — Removing Data

```sql
-- Delete a specific row
DELETE FROM users WHERE id = 4;

-- Delete rows matching a condition
DELETE FROM orders WHERE status = 'cancelled';

-- Delete all rows (leaves the table structure intact)
DELETE FROM users;
```

> **Always use a `WHERE` clause with `DELETE`.** Without it, the entire table is cleared.

---

## CREATE TABLE

```sql
CREATE TABLE products (
  id         INTEGER PRIMARY KEY AUTOINCREMENT,
  name       TEXT NOT NULL,
  price      DECIMAL(10, 2) NOT NULL,
  stock      INTEGER DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Common Data Types

| Type | Use for |
|------|---------|
| `INTEGER` / `INT` | Whole numbers |
| `DECIMAL(p, s)` | Fixed-precision numbers (e.g. money) |
| `FLOAT` / `REAL` | Approximate decimal numbers |
| `TEXT` / `VARCHAR(n)` | Strings |
| `BOOLEAN` | True/false |
| `DATE` | Date only |
| `TIMESTAMP` | Date and time |

> Data types vary slightly between databases. `AUTOINCREMENT` is SQLite; PostgreSQL uses `SERIAL` or `GENERATED ALWAYS AS IDENTITY`.

---

## ALTER and DROP

### Alter Table

```sql
-- Add a column
ALTER TABLE users ADD COLUMN phone TEXT;

-- Remove a column
ALTER TABLE users DROP COLUMN phone;

-- Rename a column
ALTER TABLE users RENAME COLUMN name TO full_name;

-- Change a column's type (PostgreSQL syntax)
ALTER TABLE users ALTER COLUMN age TYPE BIGINT;
```

### Drop Table

```sql
DROP TABLE products;                -- Permanently delete the table
DROP TABLE IF EXISTS products;      -- Only delete if it exists (no error if not)
```

---

## Constraints

Constraints enforce rules on the data in a table.

```sql
CREATE TABLE users (
  id      INTEGER PRIMARY KEY,          -- Unique, not null, used as row identifier
  email   TEXT UNIQUE NOT NULL,         -- No duplicates, required
  age     INTEGER CHECK (age >= 0),     -- Must pass a condition
  role    TEXT DEFAULT 'user',          -- Default value if none provided
  team_id INTEGER REFERENCES teams(id) ON DELETE SET NULL  -- Foreign key
);
```

| Constraint | Meaning |
|------------|---------|
| `PRIMARY KEY` | Unique identifier for each row; cannot be NULL |
| `UNIQUE` | All values in the column must be different |
| `NOT NULL` | The column cannot be empty |
| `DEFAULT value` | Value used when none is provided |
| `CHECK (expr)` | Value must satisfy the expression |
| `REFERENCES table(col)` | Foreign key — value must exist in another table |

---

## Quick Reference Cheat Sheet

| Task | SQL |
|------|-----|
| Select all | `SELECT * FROM table;` |
| Select columns | `SELECT col1, col2 FROM table;` |
| Filter rows | `WHERE condition` |
| Sort | `ORDER BY col DESC` |
| Limit rows | `LIMIT 10 OFFSET 20` |
| Count rows | `SELECT COUNT(*) FROM table;` |
| Sum values | `SELECT SUM(col) FROM table;` |
| Average | `SELECT AVG(col) FROM table;` |
| Group rows | `GROUP BY col` |
| Filter groups | `HAVING COUNT(*) > 1` |
| Join tables | `JOIN table2 ON t1.id = t2.fk` |
| All left rows | `LEFT JOIN` |
| Insert row | `INSERT INTO table (cols) VALUES (vals);` |
| Update row | `UPDATE table SET col = val WHERE id = 1;` |
| Delete row | `DELETE FROM table WHERE id = 1;` |
| Create table | `CREATE TABLE name (col type constraint);` |
| Drop table | `DROP TABLE IF EXISTS name;` |
| Add column | `ALTER TABLE t ADD COLUMN col type;` |
| Pattern match | `WHERE name LIKE 'A%'` |
| Null check | `WHERE col IS NULL` |
| Range | `WHERE age BETWEEN 18 AND 65` |
| In list | `WHERE city IN ('London', 'Paris')` |

---

*Part of the learning-resources collection.*
