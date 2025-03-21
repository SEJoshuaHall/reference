# SQL Reference Guide
By Joshua Hall, Edited by Claude.ai
### Set Operations

````sql
-- UNION: combines results of two queries and removes duplicates
SELECT id, name FROM customers
UNION
SELECT id, company_name FROM vendors;

-- UNION ALL: combines results of two queries and keeps duplicates
SELECT city FROM customers
UNION ALL
SELECT city FROM suppliers;

-- INTERSECT: returns rows common to both queries
SELECT product_id FROM order_items
INTERSECT
SELECT product_id FROM inventory WHERE stock < 10;

-- EXCEPT: returns rows from first query not in second query
SELECT customer_id FROM customers
EXCEPT
SELECT customer_id FROM orders;
```## Other Advanced Features

### WITH Clause (Common Table Expressions)

Common Table Expressions (CTEs) provide a way to create temporary result sets that exist only for a single query:

```sql
-- Simple CTE
WITH filtered_products AS (
  SELECT * FROM products WHERE category = 'Electronics' AND price > 100
)
SELECT * FROM filtered_products WHERE stock_quantity > 0;

-- Multiple CTEs
WITH category_sales AS (
  SELECT category, SUM(price * quantity) AS total_sales
  FROM products
  JOIN order_items ON products.id = order_items.product_id
  GROUP BY category
),
top_categories AS (
  SELECT category FROM category_sales
  ORDER BY total_sales DESC
  LIMIT 3
)
SELECT p.* FROM products p
JOIN top_categories tc ON p.category = tc.category;
````

### Recursive CTEs

Recursive CTEs are useful for querying hierarchical or graph-structured data:

```sql
-- Organizational hierarchy example
WITH RECURSIVE org_hierarchy AS (
  -- Base case: top-level employees with no manager
  SELECT id, name, manager_id, 1 AS level
  FROM employees
  WHERE manager_id IS NULL
  
  UNION ALL
  
  -- Recursive case: employees with managers
  SELECT e.id, e.name, e.manager_id, oh.level + 1
  FROM employees e
  JOIN org_hierarchy oh ON e.manager_id = oh.id
)
SELECT id, name, level FROM org_hierarchy ORDER BY level, id;

-- Bill of materials example
WITH RECURSIVE parts_breakdown AS (
  -- Base product
  SELECT product_id, component_id, quantity, 1 AS level
  FROM components
  WHERE product_id = 100
  
  UNION ALL
  
  -- Component parts
  SELECT c.product_id, c.component_id, c.quantity, pb.level + 1
  FROM components c
  JOIN parts_breakdown pb ON c.product_id = pb.component_id
)
SELECT * FROM parts_breakdown;
```

### Window Functions

Window functions perform calculations across related rows:

````sql
-- Row number
SELECT 
  category,
  product_name,
  price,
  ROW_NUMBER() OVER (PARTITION BY category ORDER BY price DESC) AS price_rank
FROM products;

-- Running total
SELECT
  order_date,
  amount,
  SUM(amount) OVER (ORDER BY order_date) AS running_total
FROM orders;

-- Moving average
SELECT
  order_date,
  amount,
  AVG(amount) OVER (
    ORDER BY order_date 
    ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
  ) AS three_day_avg
FROM orders;

-- First value, last value
SELECT
  category,
  product_name,
  price,
  FIRST_VALUE(product_name) OVER (
    PARTITION BY category 
    ORDER BY price DESC
  ) AS most_expensive_product
FROM products;

-- Lead and lag
SELECT
  order_date,
  amount,
  LAG(amount) OVER (ORDER BY order_date) AS prev_amount,
  amount - LAG(amount) OVER (ORDER BY order_date) AS amount_change
FROM orders;
```## Subqueries

Subqueries are queries nested within another query and can be used in various ways:

### Basic Subqueries

```sql
-- Subquery in WHERE clause
SELECT title, price FROM products
WHERE price > (SELECT AVG(price) FROM products);

-- Subquery in FROM clause (derived table)
SELECT category, avg_price
FROM (
  SELECT category, AVG(price) AS avg_price
  FROM products
  GROUP BY category
) AS category_averages
WHERE avg_price > 100;

-- Subquery with IN
SELECT name FROM customers
WHERE id IN (SELECT customer_id FROM orders WHERE total > 1000);

-- Subquery with EXISTS
SELECT name FROM products p
WHERE EXISTS (
  SELECT 1 FROM order_items oi
  WHERE oi.product_id = p.id
);

-- Correlated subquery
SELECT product_name, 
       (SELECT COUNT(*) FROM order_items oi
        WHERE oi.product_id = p.id) AS times_ordered
FROM products p;
````

### Scalar Subqueries

A scalar subquery returns exactly one row with one column (a single value) and can be used anywhere a single value expression is expected:

```sql
-- Scalar subquery in SELECT clause
SELECT 
  product_name,
  price,
  (SELECT AVG(price) FROM products) AS avg_price,
  price - (SELECT AVG(price) FROM products) AS diff_from_avg
FROM products;

-- Scalar subquery in WHERE clause
SELECT employee_name, salary
FROM employees
WHERE salary > (SELECT MAX(salary) FROM employees WHERE department = 'Marketing');

-- Scalar subquery in HAVING clause
SELECT department, AVG(salary) AS avg_dept_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > (SELECT AVG(salary) FROM employees);
```

### Row Subqueries and Row Constructors

Row subqueries return a single row with multiple columns and can be used with row constructors for concise comparisons:

```sql
-- Row constructor comparison
SELECT * FROM employees
WHERE (department, manager_id) = ('Sales', 10);

-- Row comparison with subquery
SELECT * FROM employees
WHERE (department, salary) = (
  SELECT department, MAX(salary) 
  FROM employees 
  GROUP BY department
  HAVING department = 'Sales'
);

-- Row comparison with IN
SELECT * FROM orders
WHERE (customer_id, order_date) IN (
  SELECT customer_id, MAX(order_date)
  FROM orders
  GROUP BY customer_id
);
```

### Subqueries vs. Joins

Subqueries can often be rewritten as joins and vice versa:

```sql
-- Using a JOIN
SELECT c.name
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.total > 1000;

-- Equivalent using a subquery
SELECT name FROM customers
WHERE id IN (SELECT customer_id FROM orders WHERE total > 1000);
```

Considerations for choosing between subqueries and joins:

- Joins are often more efficient for retrieving data from multiple tables
- Subqueries can be more readable for certain types of queries
- Subqueries are useful when you need to perform aggregation before joining
- Query performance may differ depending on the specific case and database implementation### Views

```sql
-- Create a view
CREATE VIEW active_users AS
SELECT * FROM users WHERE enabled = true;

-- Query a view
SELECT * FROM active_users;

-- Drop a view
DROP VIEW active_users;
```

### Common Table Expressions (CTEs)

```sql
-- Basic CTE
WITH active_users AS (
  SELECT * FROM users WHERE enabled = true
)
SELECT * FROM active_users;

-- Recursive CTE
WITH RECURSIVE subordinates AS (
  SELECT id, name, manager_id
  FROM employees
  WHERE id = 1
  UNION ALL
  SELECT e.id, e.name, e.manager_id
  FROM employees e
  JOIN subordinates s ON e.manager_id = s.id
)
SELECT * FROM subordinates;
```

### Window Functions

````sql
-- Calculate running total
SELECT
  order_date,
  amount,
  SUM(amount) OVER (ORDER BY order_date) AS running_total
FROM orders;

-- Partition data
SELECT
  department,
  employee_name,
  salary,
  AVG(salary) OVER (PARTITION BY department) AS dept_avg
FROM employees;
```# Comprehensive SQL Reference Guide

## Table of Contents
1. [SQL Fundamentals](#sql-fundamentals)
2. [SQL Sub-languages](#sql-sub-languages)
3. [PostgreSQL Data Types](#postgresql-data-types)
4. [Table Management](#table-management)
5. [Keys and Constraints](#keys-and-constraints)
6. [Data Manipulation](#data-manipulation)
7. [Query Operations](#query-operations)
8. [Table Relationships](#table-relationships)
9. [Joins](#joins)
10. [Subqueries](#subqueries)
11. [Functions](#functions)
12. [Tools and Commands](#tools-and-commands)
13. [PostgreSQL-Specific Features](#postgresql-specific-features)
14. [Database Design and Schema Levels](#database-design-and-schema-levels)
15. [Best Practices](#best-practices)

## SQL Fundamentals

### Definition
- **SQL**: A _special-purpose_, _declarative_ language used to manipulate the structure and values of datasets stored in a relational database.
- SQL statements must be terminated by a semicolon.
- `NULL` is a special value representing the absence of any other value.
- `NULL` values must be compared using `IS NULL` or `IS NOT NULL`.

### Key Concepts
- **Relational Database**: A structured collection of data organized according to the relational model.
- **RDBMS**: Relational Database Management System. A software application for managing relational databases, such as PostgreSQL.
- **Relation**: A set of individual but related data entries; analogous to a database table.
- **Schema**: The blueprint or structure of a database.

### Relationship between SQL and Spreadsheets

| Spreadsheet      | Database     |
| ---------------- | ------------ |
| Worksheet        | Table        |
| Worksheet Column | Table Column |
| Worksheet Row    | Table Record |

### Common SQL Operators

| Operator | Description |
| -------- | ----------- |
| `=`      | Tests equality |
| `<>` or `!=` | Tests inequality |
| `<`      | Less than |
| `>`      | Greater than |
| `<=`     | Less than or equal to |
| `>=`     | Greater than or equal to |
| `AND`    | Logical AND |
| `OR`     | Logical OR |
| `NOT`    | Logical NOT |
| `LIKE`   | Pattern matching with wildcards |
| `ILIKE`  | Case-insensitive pattern matching |
| `IN`     | Checks if a value is in a list of values |
| `BETWEEN` | Range check (inclusive) |
| `IS NULL` | Checks if a value is NULL |
| `IS NOT NULL` | Checks if a value is not NULL |

## SQL Sub-languages

| Sub-language | Controls | SQL Constructs |
| ------------ | -------- | -------------- |
| **DDL** (Data Definition Language) | Relation structure and rules | `CREATE`, `DROP`, `ALTER` |
| **DML** (Data Manipulation Language) | Values stored within relations | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **DCL** (Data Control Language) | Who can do what | `GRANT`, `REVOKE` |
| **TCL** (Transaction Control Language) | Transaction management | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

Understanding these sublanguages helps classify different SQL statements based on their purpose:
- DDL statements define and modify database structure
- DML statements manipulate the data within tables
- DCL statements control user access and permissions
- TCL statements manage transactions

## PostgreSQL Data Types

| Data Type | Type | Value | Example Values |
| --------- | ---- | ----- | -------------- |
| `varchar(length)` | character | Up to `length` characters of text | `canoe` |
| `text` | character | Unlimited length of text | `a long string of text` |
| `integer` or `INT` | numeric | Whole numbers | `42`, `-1423290` |
| `real` | numeric | Floating-point numbers | `24.563`, `-14924.3515` |
| `decimal(precision, scale)` | numeric | Arbitrary precision numbers | `123.45`, `-567.89` |
| `timestamp` | date/time | Date and time | `1999-01-08 04:05:06` |
| `date` | date/time | Only a date | `1999-01-08` |
| `boolean` | boolean | True or false | `true`, `false` |
| `serial` | numeric | Auto-incrementing integer | Used for IDs |
| `char(n)` | character | Fixed-length string | String padded with spaces |
| `uuid` | - | Universally unique identifier | - |
| `json`/`jsonb` | - | JSON data | JSONB is binary and indexed |
| `array` | - | Array of values | - |

> **Note**: For newer PostgreSQL versions, use `IDENTITY` instead of `SERIAL` for auto-incrementing columns.

## Table Management

### Creating Tables

```sql
CREATE TABLE users (
  id serial PRIMARY KEY,
  username varchar(25) UNIQUE NOT NULL,
  enabled boolean DEFAULT true,
  last_login timestamp
);
````

### Temporary Tables

Temporary tables exist only for the duration of a session or transaction:

```sql
-- Create a temporary table for the current session
CREATE TEMPORARY TABLE temp_results (
  id integer,
  result_value numeric,
  calculation_date timestamp DEFAULT CURRENT_TIMESTAMP
);

-- Create a temporary table that is dropped at the end of the transaction
CREATE TEMPORARY TABLE temp_calculations (
  id integer,
  calculation text
) ON COMMIT DROP;
```

### Modifying Tables

|Action|Command|Example|
|---|---|---|
|Add a column|`ALTER TABLE table_name ADD COLUMN column_name data_type CONSTRAINTS;`|`ALTER TABLE users ADD COLUMN email varchar(100) NOT NULL;`|
|Alter column data type|`ALTER TABLE table_name ALTER COLUMN column_name TYPE data_type;`|`ALTER TABLE users ALTER COLUMN username TYPE varchar(50);`|
|Set default value|`ALTER TABLE table_name ALTER COLUMN column_name SET DEFAULT value;`|`ALTER TABLE users ALTER COLUMN enabled SET DEFAULT true;`|
|Rename a table|`ALTER TABLE table_name RENAME TO new_table_name;`|`ALTER TABLE users RENAME TO accounts;`|
|Rename a column|`ALTER TABLE table_name RENAME COLUMN column_name TO new_column_name;`|`ALTER TABLE users RENAME COLUMN username TO login;`|
|Remove a column|`ALTER TABLE table_name DROP COLUMN column_name;`|`ALTER TABLE users DROP COLUMN last_login;`|
|Delete a table|`DROP TABLE table_name;`|`DROP TABLE users;`|

## Keys and Constraints

### Key Types

- **Natural key**: An existing value in a dataset that can uniquely identify each row of data (e.g., ISBN for books, SSN for people).
- **Surrogate key**: A value created solely for identifying a row of data in a database table, not derived from application data (e.g., auto-generated IDs).
- **Primary key**: A value used to uniquely identify rows in a table. It cannot be `NULL` and must be unique within a table. Created using `PRIMARY KEY`.
- **Foreign key**: A column that references the primary key of another table, establishing relationships between tables.
- **Composite key**: A key that consists of multiple columns which together uniquely identify rows.

### Adding Constraints

|Constraint|Command|Example|
|---|---|---|
|Primary Key|`ALTER TABLE table_name ADD PRIMARY KEY (column_name);`|`ALTER TABLE users ADD PRIMARY KEY (id);`|
|Foreign Key|`ALTER TABLE table_name ADD FOREIGN KEY (column_name) REFERENCES other_table(column_name);`|`ALTER TABLE orders ADD FOREIGN KEY (user_id) REFERENCES users(id);`|
|Unique|`ALTER TABLE table_name ADD UNIQUE (column_name);`|`ALTER TABLE users ADD UNIQUE (email);`|
|Check|`ALTER TABLE table_name ADD CHECK (expression);`|`ALTER TABLE products ADD CHECK (price > 0);`|
|Not Null|`ALTER TABLE table_name ALTER COLUMN column_name SET NOT NULL;`|`ALTER TABLE users ALTER COLUMN username SET NOT NULL;`|
|Default|`ALTER TABLE table_name ALTER COLUMN column_name SET DEFAULT value;`|`ALTER TABLE users ALTER COLUMN enabled SET DEFAULT true;`|

### Removing Constraints

```sql
-- Remove named constraint
ALTER TABLE table_name DROP CONSTRAINT constraint_name;

-- Remove NOT NULL constraint
ALTER TABLE table_name ALTER COLUMN column_name DROP NOT NULL;

-- Remove default value
ALTER TABLE table_name ALTER COLUMN column_name DROP DEFAULT;
```

### Auto-incrementing Columns in PostgreSQL

```sql
-- Using SERIAL (older method)
CREATE TABLE products (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL
);

-- Using IDENTITY (newer method, preferred for PostgreSQL 10+)
CREATE TABLE customers (
  id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  name VARCHAR(100) NOT NULL
);

-- Or with custom sequence settings
CREATE TABLE orders (
  id INTEGER GENERATED BY DEFAULT AS IDENTITY 
     (START WITH 1000 INCREMENT BY 1) PRIMARY KEY,
  order_date DATE NOT NULL
);
```

## Data Manipulation

### INSERT

```sql
-- Insert a single row
INSERT INTO users (username, email)
VALUES ('john_doe', 'john@example.com');

-- Insert multiple rows
INSERT INTO users (username, email)
VALUES 
  ('jane_smith', 'jane@example.com'),
  ('bob_jones', 'bob@example.com');
```

### SELECT

```sql
-- Basic select
SELECT column1, column2 FROM table_name;

-- Select with condition
SELECT username, email FROM users WHERE enabled = true;

-- Select with ordering
SELECT username, last_login FROM users ORDER BY last_login DESC;
```

### UPDATE

```sql
-- Update all rows
UPDATE users SET enabled = true;

-- Update with condition
UPDATE users SET enabled = false WHERE last_login < '2023-01-01';

-- Update multiple columns
UPDATE users SET username = 'new_username', email = 'new@example.com' WHERE id = 1;
```

### DELETE

```sql
-- Delete all rows
DELETE FROM temporary_logs;

-- Delete with condition
DELETE FROM users WHERE enabled = false;
```

## Query Operations

### WHERE Clause

```sql
-- Comparison operators
SELECT * FROM products WHERE price > 100;

-- Logical operators
SELECT * FROM users WHERE enabled = true AND last_login > '2023-01-01';

-- Pattern matching
SELECT * FROM products WHERE name LIKE 'Wireless%';

-- Using EXISTS
SELECT * FROM customers c
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.id);

-- Using IN
SELECT * FROM products WHERE category IN ('Electronics', 'Computers', 'Accessories');

-- Using NOT IN
SELECT * FROM products WHERE category NOT IN ('Clothing', 'Furniture');

-- Using ANY/SOME
SELECT * FROM products WHERE price > ANY(SELECT price FROM featured_products);

-- Using ALL
SELECT * FROM products WHERE price > ALL(SELECT price FROM budget_products);

-- Row comparison
SELECT * FROM employees
WHERE (department, manager_id) = ('Sales', 10);

-- Multiple row comparison
SELECT * FROM employees
WHERE (department, manager_id) IN (('Sales', 10), ('Marketing', 12));
```

### Scalar Subqueries

Scalar subqueries return a single value and can be used anywhere a single value expression is expected:

```sql
-- Using a scalar subquery in SELECT
SELECT 
  product_name,
  price,
  (SELECT AVG(price) FROM products) AS avg_price,
  price - (SELECT AVG(price) FROM products) AS diff_from_avg
FROM products;

-- Using a scalar subquery in WHERE
SELECT product_name, price
FROM products
WHERE price > (SELECT MAX(price) FROM products WHERE category = 'Budget');

-- Using a scalar subquery in UPDATE
UPDATE products
SET discount_price = price * 0.9
WHERE price > (SELECT AVG(price) FROM products);
```

### ORDER BY

```sql
-- Default sort (ascending)
SELECT * FROM users ORDER BY username;

-- Descending sort
SELECT * FROM products ORDER BY price DESC;

-- Multiple columns
SELECT * FROM orders ORDER BY customer_id ASC, order_date DESC;
```

### LIMIT and OFFSET

```sql
-- Limit results
SELECT * FROM products LIMIT 10;

-- Pagination
SELECT * FROM products ORDER BY id LIMIT 10 OFFSET 20;
```

### DISTINCT

```sql
-- Get unique values
SELECT DISTINCT category FROM products;

-- Count distinct values
SELECT COUNT(DISTINCT category) FROM products;
```

### GROUP BY and HAVING

```sql
-- Group data
SELECT category, COUNT(*) FROM products GROUP BY category;

-- Filter grouped data
SELECT category, COUNT(*) FROM products GROUP BY category HAVING COUNT(*) > 5;
```

## Table Relationships

### Types of Relationships

- **One-to-One**: Each record in the first table is related to exactly one record in the second table.
- **One-to-Many**: Each record in the first table can be related to multiple records in the second table.
- **Many-to-Many**: Multiple records in the first table can be related to multiple records in the second table (requires a junction/join table).

### Implementing Relationships

#### One-to-One Example

```sql
CREATE TABLE users (
  id serial PRIMARY KEY,
  username varchar(50) NOT NULL
);

CREATE TABLE user_profiles (
  user_id integer PRIMARY KEY,
  bio text,
  avatar_url varchar(255),
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

#### One-to-Many Example

```sql
CREATE TABLE authors (
  id serial PRIMARY KEY,
  name varchar(100) NOT NULL
);

CREATE TABLE books (
  id serial PRIMARY KEY,
  title varchar(255) NOT NULL,
  author_id integer NOT NULL,
  FOREIGN KEY (author_id) REFERENCES authors(id) ON DELETE CASCADE
);
```

#### Many-to-Many Example

```sql
CREATE TABLE students (
  id serial PRIMARY KEY,
  name varchar(100) NOT NULL
);

CREATE TABLE courses (
  id serial PRIMARY KEY,
  name varchar(100) NOT NULL
);

CREATE TABLE enrollments (
  student_id integer NOT NULL,
  course_id integer NOT NULL,
  enrollment_date date NOT NULL DEFAULT CURRENT_DATE,
  PRIMARY KEY (student_id, course_id),
  FOREIGN KEY (student_id) REFERENCES students(id) ON DELETE CASCADE,
  FOREIGN KEY (course_id) REFERENCES courses(id) ON DELETE CASCADE
);
```

### Referential Integrity

- Ensures relationships between tables remain consistent
- Enforced through foreign key constraints
- ON DELETE options:
    - `CASCADE`: Delete related records
    - `SET NULL`: Set the foreign key to NULL
    - `SET DEFAULT`: Set the foreign key to its default value
    - `RESTRICT` or `NO ACTION`: Prevent deletion if references exist

## Joins

### Types of Joins

```sql
-- INNER JOIN: returns rows when there is a match in both tables
SELECT books.title, authors.name
FROM books
INNER JOIN authors ON books.author_id = authors.id;

-- LEFT JOIN: returns all rows from the left table and matched rows from the right
SELECT users.username, orders.id
FROM users
LEFT JOIN orders ON users.id = orders.user_id;

-- RIGHT JOIN: returns all rows from the right table and matched rows from the left
SELECT users.username, orders.id
FROM users
RIGHT JOIN orders ON users.id = orders.user_id;

-- FULL JOIN: returns rows when there is a match in one of the tables
SELECT users.username, orders.id
FROM users
FULL JOIN orders ON users.id = orders.user_id;

-- CROSS JOIN: returns the Cartesian product of both tables
SELECT users.username, roles.name
FROM users
CROSS JOIN roles;

-- SELF JOIN: joins a table to itself
SELECT e1.name AS employee, e2.name AS manager
FROM employees e1
JOIN employees e2 ON e1.manager_id = e2.id;
```

Each type of join has specific use cases:

- INNER JOIN: When you only want records that have matching values in both tables
- LEFT JOIN: When you want all records from the first table regardless of matches
- RIGHT JOIN: When you want all records from the second table regardless of matches
- FULL JOIN: When you want all records from both tables regardless of matches
- CROSS JOIN: When you want all possible combinations of records from both tables
- SELF JOIN: When you need to relate records within the same table

### Join Tables for Many-to-Many Relationships

Join tables (junction tables) connect entities in many-to-many relationships:

```sql
-- Creating a many-to-many relationship using a join table
CREATE TABLE students (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL
);

CREATE TABLE courses (
  id SERIAL PRIMARY KEY,
  title VARCHAR(100) NOT NULL
);

-- Join table for students and courses
CREATE TABLE enrollments (
  student_id INTEGER REFERENCES students(id) ON DELETE CASCADE,
  course_id INTEGER REFERENCES courses(id) ON DELETE CASCADE,
  enrollment_date DATE NOT NULL DEFAULT CURRENT_DATE,
  grade VARCHAR(2),
  PRIMARY KEY (student_id, course_id)
);

-- Querying with a join table
SELECT s.name, c.title, e.grade
FROM students s
JOIN enrollments e ON s.id = e.student_id
JOIN courses c ON e.course_id = c.id
WHERE c.title LIKE 'Introduction%';
```

## Functions

### Aggregate Functions

|Function|Example|Description|
|---|---|---|
|`COUNT()`|`SELECT COUNT(*) FROM users;`|Returns the number of rows|
|`SUM()`|`SELECT SUM(amount) FROM orders;`|Returns the sum of values|
|`AVG()`|`SELECT AVG(price) FROM products;`|Returns the average of values|
|`MIN()`|`SELECT MIN(price) FROM products;`|Returns the minimum value|
|`MAX()`|`SELECT MAX(price) FROM products;`|Returns the maximum value|

### String Functions

|Function|Example|Description|
|---|---|---|
|`LENGTH()`|`SELECT LENGTH(username) FROM users;`|Returns the length of the string|
|`TRIM()`|`SELECT TRIM(' username ');`|Removes spaces from both ends|
|`UPPER()`|`SELECT UPPER(username) FROM users;`|Converts to uppercase|
|`LOWER()`|`SELECT LOWER(username) FROM users;`|Converts to lowercase|
|`CONCAT()`|`SELECT CONCAT(first_name, ' ', last_name);`|Combines strings|

### Date/Time Functions

|Function|Example|Description|
|---|---|---|
|`DATE_PART()`|`SELECT DATE_PART('year', created_at);`|Extracts a part from a date|
|`AGE()`|`SELECT AGE(CURRENT_DATE, birth_date);`|Calculates the difference between dates|
|`NOW()`|`SELECT NOW();`|Current date and time|
|`CURRENT_DATE`|`SELECT CURRENT_DATE;`|Current date|

## Tools and Commands

### PostgreSQL Client Applications

|Command|Description|
|---|---|
|`createdb database_name`|Creates a new database|
|`dropdb database_name`|Permanently deletes a database|
|`pg_dump -d database_name > file.sql`|Backs up a database|
|`psql -d database_name < file.sql`|Restores a database|

### psql Meta-commands

|Command|Description|
|---|---|
|`\l` or `\list`|Lists all databases|
|`\c database_name` or `\connect database_name`|Connects to a database|
|`\dt`|Shows all tables in the current database|
|`\d table_name`|Shows the schema of a table|
|`\di`|Lists all indexes in the current database|
|`\du`|Lists all users and their roles|
|`\?`|Shows help for psql commands|
|`\q`|Exits psql|
|`\i file.sql`|Executes SQL from a file|

### Importing and Exporting Data

#### Import data from a CSV file into a table

```sql
\copy my_table FROM '/path/to/data.csv' WITH (FORMAT csv, HEADER true)
```

#### Export table data to a CSV file

```sql
\copy my_table TO '/path/to/output.csv' WITH (FORMAT csv, HEADER true)
```

#### Import with specific options

```sql
\copy my_table (column1, column2, column3) FROM '/path/to/data.csv' 
WITH (FORMAT csv, HEADER true, DELIMITER ';', QUOTE '"', NULL 'NA')
```

## PostgreSQL-Specific Features

### Indexes

```sql
-- Create an index
CREATE INDEX index_name ON table_name (field_name);

-- Create a multi-column index
CREATE INDEX index_name ON table_name (field1, field2);

-- Create a unique index
CREATE UNIQUE INDEX index_name ON table_name (field_name);

-- Drop an index
DROP INDEX index_name;
```

### EXPLAIN and Query Analysis

EXPLAIN is a powerful tool for understanding how PostgreSQL executes queries:

```sql
-- View the query plan
EXPLAIN SELECT * FROM products WHERE price > 100;

-- View the query plan with execution statistics
EXPLAIN ANALYZE SELECT * FROM products WHERE price > 100;

-- More verbose output with timing information
EXPLAIN (ANALYZE, BUFFERS, VERBOSE, FORMAT JSON) 
SELECT * FROM products WHERE price > 100;
```

#### Comparing SQL Statement Performance

When optimizing queries, it's important to compare different approaches:

```sql
-- Compare a subquery vs JOIN performance
-- Approach 1: Using a JOIN
EXPLAIN ANALYZE
SELECT c.name, o.order_date
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.total_amount > 1000;

-- Approach 2: Using a subquery
EXPLAIN ANALYZE
SELECT c.name, o.order_date
FROM customers c
JOIN (
  SELECT customer_id, order_date 
  FROM orders 
  WHERE total_amount > 1000
) o ON c.id = o.customer_id;

-- Approach 3: Using EXISTS
EXPLAIN ANALYZE
SELECT c.name, 
  (SELECT o.order_date FROM orders o 
   WHERE o.customer_id = c.id AND o.total_amount > 1000
   LIMIT 1)
FROM customers c
WHERE EXISTS (
  SELECT 1 FROM orders o 
  WHERE o.customer_id = c.id AND o.total_amount > 1000
);
```

Understanding the key elements in the EXPLAIN output:

- **Cost**: Estimated computational expense (higher numbers are more expensive)
- **Rows**: Estimated number of rows output by each plan node
- **Width**: Estimated average width of rows in bytes
- **Actual Time**: Elapsed time for the operation (when using ANALYZE)
- **Scan types**: Sequential Scan, Index Scan, Bitmap Index Scan, etc.
- **Join types**: Nested Loop, Hash Join, Merge Join

### Sequences

```sql
-- Create a sequence
CREATE SEQUENCE my_sequence;

-- Get next value
SELECT nextval('my_sequence');

-- Delete sequence
DROP SEQUENCE my_sequence;
```

## Database Design and Schema Levels

### Levels of Schema

1. **Conceptual Schema**: High-level design that outlines entities and their relationships without implementation details
    
    - Focus: Business entities and relationships
    - Audience: Business stakeholders, analysts
    - Example notation: Entity-Relationship Diagram (ERD) without attributes
2. **Logical Schema**: Design focused on details of entities, attributes, and relationships but independent of specific database implementation
    
    - Focus: Detailed entities, attributes, relationships, and constraints
    - Audience: Database designers, developers
    - Example notation: Detailed ERD with normalized tables and attributes
3. **Physical Schema**: Low-level database-specific design focused on implementation details
    
    - Focus: Database-specific implementation with data types, indexes, storage parameters
    - Audience: Database administrators, performance engineers
    - Example notation: Database-specific diagrams, DDL scripts

### Entity Relationship Diagrams and Crow's Foot Notation

Entity relationship diagrams use crow's foot notation to represent relationships between entities:

**Symbols for Entities**:

- Rectangle: Entity (table)
- Oval: Attribute (column)
- Underlined text: Primary key attribute

**Crow's Foot Notation for Cardinality**:

- One-to-One: Straight line on both ends (`—|—`)
- One-to-Many: Crow's foot on "many" side (`—|—<`)
- Many-to-Many: Crow's foot on both sides (`>—<`)

**Modality (Participation) Symbols**:

- Optional participation: Circle (`○—`)
- Mandatory participation: Vertical bar (`|—`)

**Complete Notation Examples**:

- One required to one optional: `|—○`
- One required to many optional: `|—○<`
- One required to many required: `|—|<`
- Many optional to many optional: `>○—○<`

### Cardinality and Modality

**Cardinality**: Defines the numerical relationship between entities (how many)

- One-to-One (1:1): Each record in Table A is related to exactly one record in Table B
- One-to-Many (1:N): Each record in Table A can be related to multiple records in Table B
- Many-to-Many (N:M): Multiple records in Table A can be related to multiple records in Table B

**Modality**: Indicates whether a relationship is optional or mandatory (whether or not)

- Optional: The relationship doesn't have to exist (0 or more)
- Mandatory: The relationship must exist (1 or more)

Example combined as "minimum:maximum" notation:

- (0,1): Optional one (zero or one)
- (1,1): Mandatory one (exactly one)
- (0,N): Optional many (zero or more)
- (1,N): Mandatory many (one or more)

## Best Practices

### Normalization

- **First Normal Form (1NF)**: Eliminate duplicate columns and create separate tables for related data.
- **Second Normal Form (2NF)**: Meet 1NF requirements and ensure all non-key attributes depend on the entire primary key.
- **Third Normal Form (3NF)**: Meet 2NF requirements and ensure all attributes depend directly on the primary key.

### Schema Design

- Use meaningful and consistent names for tables and columns.
- Choose appropriate data types for each column.
- Implement proper constraints to maintain data integrity.
- Document your schema with comments.

### Query Optimization

#### Using Indexes Effectively

- Create indexes on columns frequently used in WHERE clauses and JOIN conditions.
- Avoid creating too many indexes, as they slow down INSERT, UPDATE, and DELETE operations.
- Consider using partial indexes for filtered queries.
- Create composite indexes for queries that filter on multiple columns.
- Use covering indexes that include all columns needed by a query.

```sql
-- Create an index for a commonly filtered column
CREATE INDEX idx_products_category ON products(category);

-- Create a composite index for queries that filter on both columns
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date);

-- Create a partial index for a common specific condition
CREATE INDEX idx_active_users ON users(last_login) WHERE active = true;
```

#### Comparing SQL Statement Performance

- Use `EXPLAIN ANALYZE` to compare the performance of different query approaches.
- Look for full table scans, which may indicate missing indexes.
- Watch for hash joins vs. nested loops (nested loops are often faster for small datasets).
- Consider the number of rows each step processes.

```sql
-- Compare performance of a JOIN vs. a subquery
EXPLAIN ANALYZE
SELECT c.name, o.id FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.total > 1000;

EXPLAIN ANALYZE
SELECT c.name, o.id FROM customers c,
(SELECT id, customer_id FROM orders WHERE total > 1000) o
WHERE c.id = o.customer_id;
```

#### General Query Optimization Tips

- Avoid using `SELECT *` and only select the columns you need.
- Use appropriate WHERE clauses to limit data early in the query.
- Consider using materialized views for complex queries that run frequently.
- Use efficient join conditions and appropriate join types.
- Avoid functions on indexed columns in WHERE clauses.

### Security

- Use parameterized queries to prevent SQL injection.
- Grant minimal privileges required for each user or role.
- Regularly audit database access and changes.
- Use encrypted connections where possible.