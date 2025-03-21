# SQL Vocabulary Guide
By Joshua Hall, Edited by Claude.ai
## Keys and Relationships

|Term|Definition|
|---|---|
|**Primary Key**|A column or group of columns that uniquely identifies each row in a table|
|**Foreign Key**|A column that creates a relationship with another table's primary key|
|**Composite Key**|A key that consists of multiple columns|
|**Natural Key**|A key based on existing data attributes that can uniquely identify rows|
|**Surrogate Key**|An artificial key created solely to serve as a primary key|
|**Candidate Key**|A column or set of columns that could potentially serve as a primary key|
|**One-to-One Relationship**|A relationship where each record in one table is related to exactly one record in another table|
|**One-to-Many Relationship**|A relationship where each record in one table can be related to multiple records in another table|
|**Many-to-Many Relationship**|A relationship where multiple records in one table can be related to multiple records in another table|
|**Junction Table**|A table used to implement a many-to-many relationship (also called a join table or bridge table)|
|**Cardinality**|The number of elements in a set; in database context, it refers to the uniqueness of values in a column or the number of related rows between tables|
|**Modality**|Indicates whether a relationship is required (mandatory) or optional|
|**Referential Integrity**|Rules that ensure relationships between tables remain consistent|

## Fundamental Concepts

|Term|Definition|
|---|---|
|**SQL**|Structured Query Language; a _special-purpose_, _declarative_ language used to manage relational databases|
|**Relational Database**|A database structured according to the relational model of data, which organizes data into tables (relations)|
|**RDBMS**|Relational Database Management System; software that manages relational databases (e.g., PostgreSQL, MySQL, Oracle)|
|**PostgreSQL**|A powerful, open-source object-relational database system|
|**SQL Statement**|A complete command to perform some action in SQL (must end with a semicolon)|
|**Query**|A specific type of SQL statement that retrieves data from the database|
|**Schema**|The blueprint or structure that defines the organization of a database|
|**Database**|A structured collection of data|

## SQL Sublanguages

|Term|Definition|
|---|---|
|**DDL (Data Definition Language)**|The subset of SQL used to define database structures (CREATE, ALTER, DROP)|
|**DML (Data Manipulation Language)**|The subset of SQL used to manipulate data within the database (SELECT, INSERT, UPDATE, DELETE)|
|**DCL (Data Control Language)**|The subset of SQL used to control access to data in the database (GRANT, REVOKE)|
|**TCL (Transaction Control Language)**|The subset of SQL used to manage transactions (COMMIT, ROLLBACK, SAVEPOINT)|

## Database Objects

|Term|Definition|
|---|---|
|**Table**|A collection of related data organized in rows and columns (also called a relation)|
|**Column**|A vertical component of a table that holds a specific type of data|
|**Row**|A horizontal component of a table that represents a complete record (also called a tuple)|
|**Field**|The intersection of a row and column; contains a specific data value|
|**Record**|Another term for a row; a complete set of related fields|
|**View**|A virtual table based on the result set of a stored query|
|**Index**|A data structure that improves the speed of data retrieval operations|
|**Sequence**|A database object that generates unique numeric values|
|**Trigger**|A procedure that automatically executes in response to certain events on a particular table|
|**Stored Procedure**|A prepared SQL code that can be saved and reused|
|**Temporary Table**|A transient table that exists only for the duration of a session or transaction|
|**Materialized View**|A physical copy of a query result stored for faster access|

## Data Types

|Term|Definition|
|---|---|
|**Varchar**|Variable-length character string with a maximum length|
|**Char**|Fixed-length character string|
|**Text**|Variable unlimited length string|
|**Integer**|Whole number without decimal places|
|**Serial**|Auto-incrementing integer (PostgreSQL specific)|
|**Decimal/Numeric**|Exact numeric with a specified precision and scale|
|**Real/Float**|Approximate numeric data types|
|**Boolean**|Logical data type that can have only true or false values|
|**Date**|Stores date values (year, month, day)|
|**Time**|Stores time values (hour, minute, second)|
|**Timestamp**|Stores both date and time values|
|**NULL**|A special marker indicating that a value is missing or unknown|

## Keys and Relationships

|Term|Definition|
|---|---|
|**Primary Key**|A column or group of columns that uniquely identifies each row in a table|
|**Foreign Key**|A column that creates a relationship with another table's primary key|
|**Composite Key**|A key that consists of multiple columns|
|**Natural Key**|A key based on existing data attributes that can uniquely identify rows|
|**Surrogate Key**|An artificial key created solely to serve as a primary key|
|**Candidate Key**|A column or set of columns that could potentially serve as a primary key|
|**One-to-One Relationship**|A relationship where each record in one table is related to exactly one record in another table|
|**One-to-Many Relationship**|A relationship where each record in one table can be related to multiple records in another table|
|**Many-to-Many Relationship**|A relationship where multiple records in one table can be related to multiple records in another table|
|**Junction Table**|A table used to implement a many-to-many relationship|
|**Cardinality**|The number of distinct values in a table column|
|**Modality**|Indicates whether a relationship is required or optional|
|**Referential Integrity**|Rules that ensure relationships between tables remain consistent|

## Constraints

|Term|Definition|
|---|---|
|**NOT NULL**|Ensures a column cannot have a NULL value|
|**UNIQUE**|Ensures all values in a column are different|
|**PRIMARY KEY**|Uniquely identifies each record in a table (combines NOT NULL and UNIQUE)|
|**FOREIGN KEY**|Establishes a link between data in two tables|
|**CHECK**|Ensures all values in a column satisfy a specific condition|
|**DEFAULT**|Provides a default value when no value is specified|
|**CONSTRAINT**|A rule enforced on data columns to ensure data integrity|
|**CASCADE**|An option that automatically propagates changes (like DELETE or UPDATE) to related tables|

## SQL Commands and Clauses

|Term|Definition|
|---|---|
|**SELECT**|Retrieves data from a database|
|**INSERT**|Adds new records to a table|
|**UPDATE**|Modifies existing records|
|**DELETE**|Removes records from a table|
|**CREATE**|Makes a new database object (table, view, etc.)|
|**ALTER**|Modifies an existing database object|
|**DROP**|Deletes an entire database object|
|**FROM**|Specifies which table(s) to retrieve data from|
|**WHERE**|Filters records based on a condition|
|**GROUP BY**|Groups rows that have the same values|
|**HAVING**|Filters for groups (used with GROUP BY)|
|**ORDER BY**|Sorts the result set|
|**JOIN**|Combines rows from two or more tables|
|**UNION**|Combines the results of two or more SELECT statements|
|**LIMIT**|Restricts the number of rows returned|
|**OFFSET**|Skips a specified number of rows|
|**AS**|Creates an alias for a column or table|
|**DISTINCT**|Returns only unique values|

## Join Types

|Term|Definition|
|---|---|
|**INNER JOIN**|Returns rows when there is a match in both tables|
|**LEFT JOIN**|Returns all rows from the left table and matched rows from the right table|
|**RIGHT JOIN**|Returns all rows from the right table and matched rows from the left table|
|**FULL JOIN**|Returns rows when there is a match in one of the tables|
|**CROSS JOIN**|Returns the Cartesian product of both tables|
|**SELF JOIN**|Joins a table to itself|

## Subqueries and Functions

|Term|Definition|
|---|---|
|**Subquery**|A query nested inside another query|
|**Scalar Subquery**|A subquery that returns exactly one column with one row (a single value)|
|**Row Subquery**|A subquery that returns exactly one row with multiple columns|
|**Column Subquery**|A subquery that returns exactly one column with multiple rows|
|**Table Subquery**|A subquery that returns multiple columns and rows|
|**Correlated Subquery**|A subquery that references columns from the outer query|
|**Derived Table**|A subquery in the FROM clause that acts as a temporary table|
|**Common Table Expression (CTE)**|A temporary named result set created with the WITH clause|
|**Aggregate Function**|Functions that perform calculations on a set of values (e.g., COUNT, SUM, AVG)|
|**Scalar Function**|Returns a single value (e.g., UPPER, LOWER, CONCAT)|
|**Window Function**|Performs calculations across rows related to the current row|
|**User-Defined Function**|Custom functions created by users|
|**Row Constructor**|Expression that creates a row value by listing multiple column values|
|**Row Comparison**|Comparing multiple columns at once using row constructors|

## Normalization and Design

|Term|Definition|
|---|---|
|**Normalization**|The process of organizing data to reduce redundancy and improve data integrity|
|**Denormalization**|The process of combining normalized tables for performance|
|**First Normal Form (1NF)**|Each table cell contains a single value, and each record is unique|
|**Second Normal Form (2NF)**|Meets 1NF and all non-key attributes depend on the entire primary key|
|**Third Normal Form (3NF)**|Meets 2NF and all attributes depend directly on the primary key|
|**Entity**|A person, place, thing, event, or concept about which data is collected|
|**Attribute**|A characteristic or property of an entity|
|**Relationship**|A connection between entities|
|**ERD**|Entity Relationship Diagram; a visual representation of entities and their relationships|
|**Conceptual Schema**|High-level design focused on identifying entities and their relationships|
|**Logical Schema**|Design focused on details without implementation specifics|
|**Physical Schema**|Low-level database-specific design focused on implementation|
|**Crow's Foot Notation**|A method for representing entity-relationship diagrams using symbols to show cardinality|
|**UML**|Unified Modeling Language; a standardized modeling language that includes database modeling|
|**Data Dictionary**|A centralized repository of information about data such as meaning, relationships, origin, usage, and format|

## SQL Performance and Optimization

|Term|Definition|
|---|---|
|**Query Plan**|The sequence of steps the database uses to execute a query|
|**EXPLAIN**|A command to show the execution plan of a query|
|**EXPLAIN ANALYZE**|Command that shows the actual execution plan with timing information|
|**Execution Time**|The time taken to execute a query|
|**Index Scan**|Using an index to locate specific rows|
|**Sequential Scan**|Reading all rows in a table sequentially|
|**Query Optimization**|The process of improving query performance|
|**Query Cache**|Stores the results of queries for faster access|
|**Materialized View**|A physical copy of a query result stored for faster access|
|**Cost**|The estimated computational expense of executing a query|
|**Statistics**|Data collected about tables and columns to help the query planner|
|**Query Planner**|The component that determines how to execute a query optimally|
|**Join Strategy**|Method used to join tables (Nested Loop, Hash Join, Merge Join)|
|**Cardinality Estimation**|Predicting how many rows will be returned by a query or operation|
|**Optimization Fence**|Operations that prevent certain optimizations from occurring|

## PostgreSQL Specific

| Term                   | Definition                                                                  |
| ---------------------- | --------------------------------------------------------------------------- |
| **psql**               | PostgreSQL's interactive terminal                                           |
| **Meta-command**       | Commands in psql that start with a backslash (e.g., \l, \d)                 |
| **Client Application** | Tools like createdb, dropdb that interact with PostgreSQL                   |
| **tablespace**         | A location where data files can be stored                                   |
| **WAL**                | Write-Ahead Logging; a method for ensuring data integrity                   |
| **TOAST**              | The Oversized-Attribute Storage Technique; used for storing large values    |
| **MVCC**               | Multi-Version Concurrency Control; allows concurrent access to the database |
| **Vacuum**             | Process that reclaims storage space and updates statistics                  |
| **Role**               | A database entity that can own database objects and have privileges         |
| **Schema Search Path** | The order in which schemas are searched                                     |