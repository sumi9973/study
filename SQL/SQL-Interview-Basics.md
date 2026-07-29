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

---

### Q1. What is the difference between `DELETE`, `TRUNCATE`, and `DROP`?

| | DELETE | TRUNCATE | DROP |
|---|---|---|---|
| **What it removes** | Specific rows (or all rows if no WHERE) | All rows | Entire table (structure + data) |
| **Can use WHERE?** | Yes | No | No |
| **Can rollback?** | Yes (it's DML) | No (it's DDL) | No (it's DDL) |
| **Speed** | Slower (logs each row) | Faster (logs minimal) | Fastest |
| **Table still exists?** | Yes | Yes | No |

**Simple answer:** "DELETE removes rows and can be rolled back. TRUNCATE removes all rows fast but cannot be rolled back. DROP removes the entire table permanently."

---

### Q2. What is the difference between `WHERE` and `HAVING`?

- **WHERE** filters rows **before** grouping happens. It works on individual rows.
- **HAVING** filters **after** `GROUP BY`. It works on groups/aggregates.

**Simple rule:** If you're filtering using `COUNT()`, `SUM()`, `AVG()` etc., use `HAVING`. For everything else, use `WHERE`.

```sql
-- WHERE: filters individual rows before grouping
SELECT city, COUNT(*) FROM customers
WHERE city != 'Delhi'       -- remove Delhi rows first
GROUP BY city;

-- HAVING: filters the grouped result
SELECT city, COUNT(*) FROM customers
GROUP BY city
HAVING COUNT(*) > 5;        -- only cities with more than 5 customers
```

**Interview one-liner:** "WHERE filters rows before grouping; HAVING filters groups after GROUP BY."

---

### Q3. What are the different types of JOINs? Explain INNER vs LEFT JOIN.

There are 5 main types:

| Join | What it returns |
|---|---|
| INNER JOIN | Only rows that have a match in **both** tables |
| LEFT JOIN | All rows from the left table + matched rows from right (NULL if no match) |
| RIGHT JOIN | All rows from the right table + matched rows from left (NULL if no match) |
| FULL OUTER JOIN | All rows from both tables (NULL where no match on either side) |
| CROSS JOIN | Every combination of rows from both tables |

**INNER vs LEFT — simple example:**

Suppose you have 3 customers and only 2 of them have placed orders.
- `INNER JOIN` → returns only those 2 customers (the ones with orders).
- `LEFT JOIN` → returns all 3 customers; the one with no orders shows NULL in order columns.

**Use LEFT JOIN when:** you want to see all records from the main table even if there's no related data.

**Interview one-liner:** "INNER JOIN returns only matching rows. LEFT JOIN returns all rows from the left table and matching rows from the right, with NULLs where there's no match."

---

### Q4. What is a Primary Key vs a Foreign Key?

**Primary Key:**
- Uniquely identifies each row in a table.
- Cannot be NULL.
- A table can have only one primary key.
- Example: `customer_id` in the `customers` table.

**Foreign Key:**
- A column in one table that refers to the Primary Key of another table.
- It creates a link (relationship) between two tables.
- Example: `customer_id` in the `orders` table pointing to `customers`.

```sql
CREATE TABLE orders (
    order_id    INT PRIMARY KEY,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

**Interview one-liner:** "A primary key uniquely identifies a row in its own table. A foreign key is a reference to a primary key in another table, used to create a relationship between them."

---

### Q5. What is the difference between SQL and NoSQL?

| | SQL | NoSQL |
|---|---|---|
| Data stored as | Tables (rows & columns) | Documents, key-value pairs, graphs, etc. |
| Schema | Fixed (must define structure first) | Flexible (each record can be different) |
| Scaling | Vertical (add more power to one server) | Horizontal (add more servers) |
| Best for | Structured data, banking, ERP, reporting | Big/fast/unstructured data, social media, IoT |
| Examples | MySQL, PostgreSQL, Oracle | MongoDB, Redis, Cassandra, DynamoDB |

**Simple analogy:**
- SQL is like an Excel sheet — strict columns, every row must fit.
- NoSQL is like a folder of JSON documents — each document can have different fields.

**When to choose SQL:** Data is structured and relationships matter (e.g., bank transactions).
**When to choose NoSQL:** Data is huge, changes often, or doesn't fit a fixed structure (e.g., user activity logs).

---

### Q6. What is Normalization? Explain 1NF, 2NF, 3NF.

**Normalization** = organizing data into multiple related tables to **remove redundancy** (duplicate data) and ensure **data integrity** (accuracy).

**Simple analogy:** Instead of writing a customer's full name and address in every order row (redundant), store the customer once in a `customers` table and just reference their ID in `orders`.

| Normal Form | Rule |
|---|---|
| **1NF** | Each column holds one single (atomic) value. No repeating groups or arrays in a cell. |
| **2NF** | Meets 1NF + every non-key column depends on the **whole** primary key (no partial dependency). |
| **3NF** | Meets 2NF + no non-key column depends on another non-key column (no transitive dependency). |

**Example for 1NF violation:** Storing `"Mumbai, Pune"` in a single `city` column — this breaks 1NF because one cell has multiple values.

**Interview one-liner:** "Normalization is the process of structuring a database to minimize redundancy and ensure data consistency, typically achieved through 1NF, 2NF, and 3NF rules."

---

### Q7. What is an Index and why is it useful?

An **index** is a separate data structure the database maintains to speed up lookups on a column.

**Simple analogy:** Like the index at the back of a textbook — instead of reading all 500 pages to find "normalization", you go to the index and jump directly to page 312.

```sql
CREATE INDEX idx_city ON customers(city);
-- Now: SELECT * FROM customers WHERE city = 'Mumbai' is much faster
```

**Pros:**
- Faster `SELECT` and search queries.

**Cons:**
- Extra storage space used.
- `INSERT`, `UPDATE`, `DELETE` become slightly slower because the index must also be updated.

**Interview one-liner:** "An index speeds up data retrieval by allowing the database to find rows without scanning the whole table, at the cost of extra storage and slightly slower writes."

---

### Q8. What is the difference between `UNION` and `UNION ALL`?

Both combine the results of two `SELECT` queries vertically (stack them on top of each other).

| | UNION | UNION ALL |
|---|---|---|
| Removes duplicates? | **Yes** | No (keeps all rows) |
| Speed | Slower (has to compare for duplicates) | **Faster** |

```sql
-- UNION: combines and removes duplicates
SELECT city FROM customers
UNION
SELECT city FROM suppliers;

-- UNION ALL: combines and keeps duplicates
SELECT city FROM customers
UNION ALL
SELECT city FROM suppliers;
```

**Rule:** Both queries must have the same number of columns with compatible data types.

**Interview one-liner:** "UNION combines results from two queries and removes duplicates. UNION ALL also combines but keeps all rows including duplicates, making it faster."

---

### Q9. What is the difference between `CHAR` and `VARCHAR`?

Both store text, but they handle length differently:

| | CHAR | VARCHAR |
|---|---|---|
| Length | Fixed — always uses the declared size | Variable — uses only as much space as needed |
| Example | `CHAR(10)` storing "Hi" uses 10 characters (padded with spaces) | `VARCHAR(10)` storing "Hi" uses 2 characters |
| Best for | Data with consistent length (e.g., country code `IN`, `US`) | Data with varying length (e.g., names, emails) |

**Interview one-liner:** "CHAR is fixed-length and pads unused space; VARCHAR is variable-length and uses only the space needed. Use CHAR for fixed-size data like country codes, VARCHAR for names and emails."

---

### Q10. What order are SQL clauses logically executed in?

Even though you *write* SELECT first, the database *executes* in this order:

```
1. FROM        — which table(s) to read
2. WHERE       — filter rows
3. GROUP BY    — group filtered rows
4. HAVING      — filter groups
5. SELECT      — pick columns / calculate
6. ORDER BY    — sort the result
7. LIMIT       — cut down to N rows
```

**Why it matters:** This explains why you cannot use a SELECT alias in a WHERE clause (alias doesn't exist yet at that point), but you can use it in ORDER BY (alias is known by then).

```sql
-- This will FAIL — alias 'total' does not exist at WHERE stage
SELECT SUM(amount) AS total FROM orders WHERE total > 1000;  -- ERROR

-- This works — use HAVING for aggregate conditions
SELECT customer_id, SUM(amount) AS total
FROM orders
GROUP BY customer_id
HAVING SUM(amount) > 1000;
```

---

### Final Tip for the Interview
- Always explain with a small real-world example (customers/orders is a safe, clear one).
- If unsure, say what you *do* know and reason out loud — interviewers value clear thinking.
- Practice writing 4–5 queries by hand (a SELECT with JOIN, a GROUP BY, an INSERT, an UPDATE with WHERE).

```