# SQL Interview Prep — Basics to Intermediate

> A beginner-friendly guide. Every topic is explained in simple language with examples you can repeat in an interview.

---

## Table of Contents
1. [What is a Database](#1-what-is-a-database)
2. [What is SQL](#2-what-is-sql)
3. [SQL vs NoSQL](#3-sql-vs-nosql)
4. [Key Terms You Must Know](#4-key-terms-you-must-know)
5. [Keys (Primary, Foreign, etc.)](#5-keys)
6. [DDL, DML, DQL, DCL, TCL Commands](#6-sql-command-categories)
7. [Basic SELECT Queries](#7-basic-select-queries)
8. [Filtering, Sorting, Grouping](#8-filtering-sorting-grouping)
9. [Joins (Very Important)](#9-joins)
10. [Aggregate Functions](#10-aggregate-functions)
11. [Constraints](#11-constraints)
12. [Normalization (Intermediate)](#12-normalization)
13. [Indexes (Intermediate)](#13-indexes)
14. [Subqueries](#14-subqueries)
15. [Common Interview Questions](#15-common-interview-questions)

---

## 1. What is a Database

A **database** is an organized collection of data stored electronically so it can be easily accessed, managed, and updated.

**Simple analogy:** Think of a database like a digital filing cabinet. Instead of paper files in drawers, you store data in tables (rows and columns), and you can search it instantly.

- A **DBMS (Database Management System)** is the software that lets you create, read, update, and delete data in a database. Examples: MySQL, PostgreSQL, Oracle, SQL Server.
- An **RDBMS (Relational DBMS)** stores data in **tables** that can be related to each other. This is the most common type.

**Interview one-liner:** "A database is an organized collection of data, and a DBMS is the software used to manage that data."

---

## 2. What is SQL

**SQL** stands for **Structured Query Language**. It is the standard language used to talk to a relational database — to store, retrieve, update, and delete data.

**Simple analogy:** If the database is a warehouse, SQL is the language you use to tell workers "give me all boxes labeled 'shoes' that arrived this week."

Example:
```sql
SELECT name, email FROM customers WHERE city = 'Mumbai';
```
This means: "Get the name and email of all customers who live in Mumbai."

**Interview one-liner:** "SQL is a standard language used to communicate with and manage data in relational databases."

---

## 3. SQL vs NoSQL

| Feature | SQL (Relational) | NoSQL (Non-relational) |
|---|---|---|
| **Structure** | Tables with rows & columns | Documents, key-value, graph, wide-column |
| **Schema** | Fixed schema (defined in advance) | Flexible / dynamic schema |
| **Scaling** | Scales vertically (bigger server) | Scales horizontally (more servers) |
| **Best for** | Structured data, complex queries, transactions | Large volumes of unstructured / rapidly changing data |
| **Examples** | MySQL, PostgreSQL, Oracle, SQL Server | MongoDB, Cassandra, Redis, DynamoDB |
| **Language** | SQL | Varies by database |

**Simple way to explain:**
- **SQL** = strict and organized, like an Excel sheet where every row must have the same columns. Great when data structure is stable (banking, ERP).
- **NoSQL** = flexible, like a folder of JSON documents that can each look different. Great for big, changing data (social media feeds, IoT).

**Interview one-liner:** "SQL databases are relational with a fixed schema and are best for structured data and complex queries, while NoSQL databases are non-relational with a flexible schema and scale better for large, unstructured data."

---

## 4. Key Terms You Must Know

- **Table:** A collection of related data organized in rows and columns (like an Excel sheet).
- **Row (Record / Tuple):** A single entry in a table (e.g., one customer).
- **Column (Field / Attribute):** A single piece of data for all rows (e.g., "email").
- **Schema:** The blueprint/structure of the database (which tables exist, their columns, and relationships).
- **Query:** A request you send to the database (usually to fetch data).

---

## 5. Keys

Keys uniquely identify rows and define relationships between tables.

- **Primary Key:** A column (or set of columns) that uniquely identifies each row. Cannot be NULL and must be unique. Example: `customer_id`.
- **Foreign Key:** A column that links to the Primary Key of another table. It creates the relationship. Example: `customer_id` in an `orders` table points to `customers`.
- **Unique Key:** Ensures all values in a column are different (but unlike primary key, can allow one NULL).
- **Composite Key:** A primary key made up of two or more columns together.
- **Candidate Key:** A column that *could* be a primary key.

**Interview one-liner:** "A primary key uniquely identifies each record in a table; a foreign key is used to link two tables together."

---

## 6. SQL Command Categories

SQL commands are grouped into 5 categories. This is a **very common interview question**.

### DDL — Data Definition Language (defines the structure)
Used to create and modify the database structure.
```sql
-- CREATE: make a new table
CREATE TABLE customers (
    customer_id   INT PRIMARY KEY,
    name          VARCHAR(100) NOT NULL,
    email         VARCHAR(100) UNIQUE,
    city          VARCHAR(50),
    created_at    DATE
);

-- ALTER: change an existing table
ALTER TABLE customers ADD phone VARCHAR(15);
ALTER TABLE customers DROP COLUMN phone;

-- DROP: delete the whole table (structure + data)
DROP TABLE customers;

-- TRUNCATE: delete all rows but keep the table structure
TRUNCATE TABLE customers;
```

### DML — Data Manipulation Language (works with the data)
Used to add, change, and remove data.
```sql
-- INSERT: add a new row
INSERT INTO customers (customer_id, name, email, city, created_at)
VALUES (1, 'Asha Rao', 'asha@example.com', 'Mumbai', '2026-01-15');

-- UPDATE: change existing data (ALWAYS use WHERE, or you update all rows!)
UPDATE customers SET city = 'Pune' WHERE customer_id = 1;

-- DELETE: remove rows (ALWAYS use WHERE, or you delete everything!)
DELETE FROM customers WHERE customer_id = 1;
```

### DQL — Data Query Language (reads the data)
```sql
SELECT * FROM customers;
```

### DCL — Data Control Language (permissions)
```sql
GRANT SELECT ON customers TO some_user;
REVOKE SELECT ON customers FROM some_user;
```

### TCL — Transaction Control Language (managing transactions)
```sql
COMMIT;      -- save changes permanently
ROLLBACK;    -- undo changes since last commit
SAVEPOINT sp1;  -- set a point you can roll back to
```

**Quick memory tip — DELETE vs TRUNCATE vs DROP:**
- `DELETE` — removes rows (can use WHERE, can be rolled back). It's DML.
- `TRUNCATE` — removes ALL rows fast, keeps table. It's DDL.
- `DROP` — removes the entire table (structure + data). It's DDL.

---

## 7. Basic SELECT Queries

```sql
-- Get all columns
SELECT * FROM customers;

-- Get specific columns
SELECT name, city FROM customers;

-- Rename a column in the output (alias)
SELECT name AS customer_name FROM customers;

-- Get unique values only
SELECT DISTINCT city FROM customers;

-- Limit number of rows
SELECT * FROM customers LIMIT 10;
```

---

## 8. Filtering, Sorting, Grouping

```sql
-- WHERE: filter rows
SELECT * FROM customers WHERE city = 'Mumbai';

-- Comparison & logical operators
SELECT * FROM customers WHERE city = 'Mumbai' AND created_at > '2026-01-01';
SELECT * FROM customers WHERE city IN ('Mumbai', 'Pune', 'Delhi');
SELECT * FROM customers WHERE name LIKE 'A%';      -- names starting with A
SELECT * FROM customers WHERE city IS NULL;         -- missing city

-- ORDER BY: sort results
SELECT * FROM customers ORDER BY name ASC;          -- A to Z
SELECT * FROM customers ORDER BY created_at DESC;   -- newest first

-- GROUP BY: group rows and summarize
SELECT city, COUNT(*) AS total_customers
FROM customers
GROUP BY city;

-- HAVING: filter AFTER grouping (WHERE can't be used with aggregates)
SELECT city, COUNT(*) AS total
FROM customers
GROUP BY city
HAVING COUNT(*) > 5;
```

**WHERE vs HAVING (common question):** `WHERE` filters individual rows *before* grouping. `HAVING` filters groups *after* `GROUP BY`.

**Order of writing a full query (important):**
```sql
SELECT column(s)
FROM table
WHERE condition
GROUP BY column
HAVING group_condition
ORDER BY column
LIMIT n;
```

---

## 9. Joins

**Joins combine rows from two or more tables based on a related column.** This is one of the most asked interview topics.

Imagine two tables:

**customers**
| customer_id | name |
|---|---|
| 1 | Asha |
| 2 | Ravi |
| 3 | Meena |

**orders**
| order_id | customer_id | amount |
|---|---|---|
| 101 | 1 | 500 |
| 102 | 1 | 300 |
| 103 | 2 | 700 |

### INNER JOIN — only matching rows in both tables
```sql
SELECT c.name, o.order_id, o.amount
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id;
```
Result: Asha (2 orders) and Ravi (1 order). **Meena is excluded** because she has no orders.

### LEFT JOIN (LEFT OUTER JOIN) — all rows from left table + matches from right
```sql
SELECT c.name, o.order_id
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id;
```
Result: Asha, Ravi, **and Meena** (with NULL order). Keeps all customers even without orders.

### RIGHT JOIN (RIGHT OUTER JOIN) — all rows from right table + matches from left
```sql
SELECT c.name, o.order_id
FROM customers c
RIGHT JOIN orders o ON c.customer_id = o.customer_id;
```
Result: all orders, even if a matching customer is missing.

### FULL OUTER JOIN — all rows from both tables (matched where possible)
```sql
SELECT c.name, o.order_id
FROM customers c
FULL OUTER JOIN orders o ON c.customer_id = o.customer_id;
```
(Note: MySQL doesn't support FULL OUTER JOIN directly; PostgreSQL/SQL Server do.)

### CROSS JOIN — every row of one table combined with every row of the other
Rarely used on purpose. Produces (rows in A × rows in B) results.

### SELF JOIN — a table joined to itself
Useful for hierarchies like employee → manager.
```sql
SELECT e.name AS employee, m.name AS manager
FROM employees e
JOIN employees m ON e.manager_id = m.employee_id;
```

**Visual memory aid:**
- INNER = intersection (only matches)
- LEFT = everything on left + matches
- RIGHT = everything on right + matches
- FULL = everything from both

---

## 10. Aggregate Functions

Functions that calculate a single value from many rows.
```sql
SELECT COUNT(*)      FROM orders;          -- number of rows
SELECT SUM(amount)   FROM orders;          -- total
SELECT AVG(amount)   FROM orders;          -- average
SELECT MIN(amount)   FROM orders;          -- smallest
SELECT MAX(amount)   FROM orders;          -- largest
```
Often combined with `GROUP BY`:
```sql
SELECT customer_id, SUM(amount) AS total_spent
FROM orders
GROUP BY customer_id;
```

---

## 11. Constraints

Rules applied to columns to keep data valid.
- **NOT NULL** — column cannot be empty.
- **UNIQUE** — no duplicate values.
- **PRIMARY KEY** — NOT NULL + UNIQUE combined.
- **FOREIGN KEY** — must match a value in another table.
- **CHECK** — value must meet a condition (e.g., `age >= 18`).
- **DEFAULT** — a default value if none is provided.

```sql
CREATE TABLE employees (
    id      INT PRIMARY KEY,
    name    VARCHAR(50) NOT NULL,
    age     INT CHECK (age >= 18),
    status  VARCHAR(10) DEFAULT 'active'
);
```

---

## 12. Normalization

**Normalization** is the process of organizing data to reduce **redundancy** (duplicate data) and improve **data integrity**.

Simple idea: instead of repeating the same customer name in every order row, store customers in one table and orders in another, then link them with a key.

- **1NF (First Normal Form):** Each column holds a single (atomic) value; no repeating groups.
- **2NF:** Is in 1NF + every non-key column depends on the *whole* primary key.
- **3NF:** Is in 2NF + no column depends on another non-key column (no "transitive" dependency).

**Interview one-liner:** "Normalization organizes data into related tables to remove redundancy and keep data consistent."

**Denormalization** is the opposite — intentionally adding some redundancy to make reads faster.

---

## 13. Indexes

An **index** is like the index at the back of a book — it helps the database find rows faster without scanning the whole table.

```sql
CREATE INDEX idx_city ON customers(city);
```

- **Pro:** Faster SELECT / search.
- **Con:** Slightly slower INSERT/UPDATE/DELETE (index must be updated) and uses extra storage.

**Interview one-liner:** "An index speeds up data retrieval, similar to a book's index, at the cost of extra storage and slower writes."

---

## 14. Subqueries

A **subquery** is a query inside another query.
```sql
-- Find customers who spent more than the average order amount
SELECT name
FROM customers
WHERE customer_id IN (
    SELECT customer_id
    FROM orders
    WHERE amount > (SELECT AVG(amount) FROM orders)
);
```

---

## 15. Common Interview Questions

Practice answering these out loud:

1. What is the difference between `DELETE`, `TRUNCATE`, and `DROP`?
2. What is the difference between `WHERE` and `HAVING`?
3. What are the different types of joins? Explain INNER vs LEFT JOIN.
4. What is a primary key vs a foreign key?
5. What is the difference between SQL and NoSQL?
6. What is normalization? Explain 1NF, 2NF, 3NF.
7. What is an index and why is it useful?
8. What is the difference between `UNION` and `UNION ALL`?
   - `UNION` combines results and removes duplicates; `UNION ALL` keeps duplicates (and is faster).
9. What is the difference between `CHAR` and `VARCHAR`?
   - `CHAR` is fixed-length; `VARCHAR` is variable-length.
10. What order are SQL clauses logically executed in?
    - `FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT`

---

### Final Tip for the Interview
- Always explain with a small real-world example (customers/orders is a safe, clear one).
- If unsure, say what you *do* know and reason out loud — interviewers value clear thinking.
- Practice writing 4–5 queries by hand (a SELECT with JOIN, a GROUP BY, an INSERT, an UPDATE with WHERE).

Good luck! 🎯
```

> *Note: This is AI-generated study material. Please double-check syntax against your target database (MySQL/PostgreSQL/etc.) as small differences exist between them.*
