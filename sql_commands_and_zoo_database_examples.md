# SQL Commands and Zoo Database Examples
By Joshua Hall, Edited by Claude.ai

### Entity Relationship Diagrams and Crow's Foot Notation

Creating an ERD for the zoo database using Crow's Foot Notation would represent the relationships as follows:

1. Between animals and species (Many-to-One):
    
    - Animals side: Many (crow's foot) - Each species can have multiple animals
    - Species side: One (single line) - Each animal belongs to exactly one species
    - Modality: Mandatory on both sides (vertical bars)
2. Between species and habitats (Many-to-One):
    
    - Species side: Many (crow's foot) - Each habitat can house multiple species
    - Habitat side: One (single line) - Each species has one primary habitat
    - Modality: Mandatory for species (vertical bar), optional for habitats (circle)
3. Between zookeepers and habitats through assignments (Many-to-Many):
    
    - Requires a junction table (assignments)
    - Zookeepers to assignments: One-to-Many
    - Habitats to assignments: One-to-Many
4. Between animals and health_records (One-to-Many):
    
    - Animals side: One (single line) - Each health record belongs to one animal
    - Health records side: Many (crow's foot) - Each animal can have multiple health records
    - Modality: Mandatory for health records, optional for animals

Sample diagram notation for animals to species relationship:

```
animals —|——|—< species
```

This represents a mandatory one (species) to mandatory many (animals) relationship.

Complete ERD would show all entities as rectangles with attributes listed inside, and relationships shown with the appropriate crow's foot notation lines between them.### EXPLAIN and Query Performance Comparison

````sql
-- Let's compare different ways to find endangered animals

-- Approach 1: Simple join
EXPLAIN ANALYZE
SELECT a.name, a.species, s.conservation_status
FROM animals a
JOIN species s ON a.species_id = s.id
WHERE s.conservation_status IN ('Endangered', 'Critically Endangered');

-- Approach 2: Using a subquery in WHERE
EXPLAIN ANALYZE
SELECT a.name, a.species, 
       (SELECT conservation_status FROM species WHERE id = a.species_id) AS status
FROM animals a
WHERE a.species_id IN (
    SELECT id FROM species 
    WHERE conservation_status IN ('Endangered', 'Critically Endangered')
);

-- Approach 3: Using EXISTS
EXPLAIN ANALYZE
SELECT a.name, a.species, s.conservation_status
FROM animals a
JOIN species s ON a.species_id = s.id
WHERE EXISTS (
    SELECT 1 FROM species s2
    WHERE s2.id = a.species_id
    AND s2.conservation_status IN ('Endangered', 'Critically Endangered')
);

-- Create an index to improve performance
CREATE INDEX idx_species_conservation ON species(conservation_status);

-- Run the queries again to see the difference
EXPLAIN ANALYZE
SELECT a.name, a.species, s.conservation_status
FROM animals a
JOIN species s ON a.species_id = s.id
WHERE s.conservation_status IN ('Endangered', 'Critically Endangered');

-- Comparing performance for different types of joins

-- Finding animals and their health records
-- Using INNER JOIN
EXPLAIN ANALYZE
SELECT a.name, hr.check_date, hr.weight
FROM animals a
INNER JOIN health_records hr ON a.id = hr.animal_id
WHERE a.species_id = 1;

-- Using LEFT JOIN with subquery
EXPLAIN ANALYZE
SELECT a.name, hr.check_date, hr.weight
FROM (SELECT * FROM animals WHERE species_id = 1) a
LEFT JOIN health_records hr ON a.id = hr.animal_id;

-- Analyze the output to determine which method is more efficient
-- Look for:
-- 1. Total execution time
-- 2. Types of scans used (Sequential vs. Index)
-- 3. Number of rows processed at each step
-- 4. Join strategies (Nested Loop, Hash Join, Merge Join)
```### Sequence Examples

```sql
-- Create a sequence for animal IDs
CREATE SEQUENCE animal_id_seq
    START WITH 100
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

-- Use the sequence to insert a new animal
INSERT INTO animals (
    id, name, species, birth_date, gender, acquisition_date, status, species_id
) VALUES (
    nextval('animal_id_seq'),
    'Fluffy',
    'Lion',
    '2021-05-12',
    'F',
    '2022-01-15',
    'Active',
    1
);

-- Get the current value of the sequence
SELECT currval('animal_id_seq');

-- Reset the sequence to a specific value
ALTER SEQUENCE animal_id_seq RESTART WITH 200;

-- Create an auto-incrementing column using SERIAL
CREATE TABLE exhibits (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    opening_date DATE NOT NULL
);

-- Create an auto-incrementing column using IDENTITY (PostgreSQL 10+)
CREATE TABLE events (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    event_date DATE NOT NULL,
    description TEXT
);

-- Manually setting sequence values (when needed)
SELECT setval('exhibits_id_seq', 100);

-- Drop a sequence
DROP SEQUENCE animal_id_seq;
```### Row Comparison

```sql
-- Create a table for animal tracking
CREATE TABLE animal_tracking (
    animal_id INTEGER REFERENCES animals(id),
    location_x DECIMAL(10, 2),
    location_y DECIMAL(10, 2),
    track_time TIMESTAMP,
    PRIMARY KEY (animal_id, track_time)
);

-- Insert sample tracking data
INSERT INTO animal_tracking VALUES
    (1, 45.2, 22.1, '2023-01-05 08:30:00'),
    (1, 46.5, 23.8, '2023-01-05 10:15:00'),
    (1, 48.1, 25.2, '2023-01-05 12:45:00'),
    (2, 32.6, 15.3, '2023-01-05 09:00:00'),
    (2, 34.1, 17.2, '2023-01-05 11:30:00');

-- Using row comparison to find specific coordinates
SELECT animal_id, track_time
FROM animal_tracking
WHERE (location_x, location_y) = (46.5, 23.8);

-- Using row comparison with IN
SELECT a.name, at.track_time
FROM animals a
JOIN animal_tracking at ON a.id = at.animal_id
WHERE (at.location_x, at.location_y) IN (
    (45.2, 22.1),
    (32.6, 15.3)
);

-- Row comparison with operators
SELECT a.name, at1.track_time, at1.location_x, at1.location_y
FROM animals a
JOIN animal_tracking at1 ON a.id = at1.animal_id
WHERE EXISTS (
    SELECT 1 
    FROM animal_tracking at2
    WHERE at2.animal_id = at1.animal_id
    AND at2.track_time <> at1.track_time
    AND (at2.location_x, at2.location_y) > (at1.location_x, at1.location_y)
);

-- Finding the latest position for each animal
SELECT DISTINCT ON (animal_id) 
    animal_id,
    location_x,
    location_y,
    track_time
FROM animal_tracking
ORDER BY animal_id, track_time DESC;
```### Scalar Subqueries

```sql
-- Using a scalar subquery to find animals above average weight
SELECT a.name, hr.weight
FROM animals a
JOIN health_records hr ON a.id = hr.animal_id
WHERE hr.weight > 
    (SELECT AVG(weight) FROM health_records)
ORDER BY hr.weight DESC;

-- Using a scalar subquery in the SELECT clause
SELECT 
    a.name,
    hr.weight,
    (SELECT AVG(weight) FROM health_records) AS avg_weight,
    hr.weight - (SELECT AVG(weight) FROM health_records) AS weight_diff
FROM animals a
JOIN health_records hr ON a.id = hr.animal_id
ORDER BY weight_diff DESC;

-- Using a scalar subquery in an UPDATE statement
UPDATE feeding_schedule
SET amount = amount * 1.1
WHERE animal_id IN (
    SELECT id 
    FROM animals 
    WHERE species_id = (
        SELECT id 
        FROM species 
        WHERE name = 'Elephant'
    )
);
```### Creating Temporary Tables

```sql
-- Create a temporary table for analyzing endangered animal data
CREATE TEMPORARY TABLE temp_endangered_animals AS
SELECT a.id, a.name, a.gender, s.name AS species_name, s.conservation_status
FROM animals a
JOIN species s ON a.species_id = s.id
WHERE s.conservation_status IN ('Endangered', 'Critically Endangered');

-- Query the temporary table
SELECT * FROM temp_endangered_animals;

-- Create a temporary table that is dropped at the end of the transaction
BEGIN;
CREATE TEMPORARY TABLE temp_calculations (
  animal_id INTEGER,
  weight_change NUMERIC(10, 2)
) ON COMMIT DROP;

INSERT INTO temp_calculations
SELECT 
  hr1.animal_id,
  hr2.weight - hr1.weight AS weight_change
FROM health_records hr1
JOIN health_records hr2 ON hr1.animal_id = hr2.animal_id
WHERE hr2.check_date > hr1.check_date
AND hr1.animal_id IN (1, 2, 3);

SELECT * FROM temp_calculations;
COMMIT;
-- After COMMIT, temp_calculations is automatically dropped
```# SQL Commands and Zoo Database Examples

## Part 1: SQL Commands Reference Charts

### Data Definition Language (DDL)

| Command | Purpose | Basic Syntax |
| ------- | ------- | ------------ |
| CREATE DATABASE | Creates a new database | `CREATE DATABASE database_name;` |
| DROP DATABASE | Deletes an entire database | `DROP DATABASE database_name;` |
| CREATE TABLE | Creates a new table | `CREATE TABLE table_name (column_definitions);` |
| ALTER TABLE | Modifies an existing table | `ALTER TABLE table_name action;` |
| DROP TABLE | Deletes a table | `DROP TABLE table_name;` |
| CREATE INDEX | Creates an index | `CREATE INDEX index_name ON table_name (column_name);` |
| DROP INDEX | Removes an index | `DROP INDEX index_name;` |
| CREATE VIEW | Creates a virtual table | `CREATE VIEW view_name AS SELECT ...;` |
| DROP VIEW | Removes a view | `DROP VIEW view_name;` |
| TRUNCATE TABLE | Removes all records from a table | `TRUNCATE TABLE table_name;` |
| CREATE SEQUENCE | Creates a sequence generator | `CREATE SEQUENCE sequence_name;` |
| CREATE TEMPORARY TABLE | Creates a temporary table | `CREATE TEMPORARY TABLE table_name (column_definitions);` |

### Data Manipulation Language (DML)

| Command | Purpose | Basic Syntax |
| ------- | ------- | ------------ |
| SELECT | Retrieves data from the database | `SELECT columns FROM table_name [WHERE condition];` |
| INSERT | Adds new records | `INSERT INTO table_name (columns) VALUES (values);` |
| UPDATE | Modifies existing records | `UPDATE table_name SET column = value [WHERE condition];` |
| DELETE | Removes records | `DELETE FROM table_name [WHERE condition];` |

### Query Clauses and Operators

| Clause/Operator | Purpose | Basic Syntax |
| --------------- | ------- | ------------ |
| WHERE | Filters records | `WHERE condition` |
| AND/OR/NOT | Combines conditions | `WHERE condition1 AND condition2` |
| IN | Checks for values in a list | `WHERE column IN (value1, value2, ...)` |
| BETWEEN | Checks for values in a range | `WHERE column BETWEEN value1 AND value2` |
| LIKE | Pattern matching | `WHERE column LIKE pattern` |
| ORDER BY | Sorts the result | `ORDER BY column [ASC|DESC]` |
| GROUP BY | Groups rows by column values | `GROUP BY column` |
| HAVING | Filters groups | `HAVING condition` |
| LIMIT | Limits the number of rows returned | `LIMIT number [OFFSET number]` |
| DISTINCT | Returns unique values | `SELECT DISTINCT column FROM table` |
| EXISTS | Checks if a subquery returns any rows | `WHERE EXISTS (subquery)` |
| NOT EXISTS | Checks if a subquery returns no rows | `WHERE NOT EXISTS (subquery)` |
| ANY/SOME | Compares a value to each value in a list | `WHERE column operator ANY (subquery)` |
| ALL | Compares a value to all values in a list | `WHERE column operator ALL (subquery)` |
| Row Constructor | Creates a row value for comparison | `WHERE (column1, column2) = (value1, value2)` |

### Joins

| Join Type | Purpose | Basic Syntax |
| --------- | ------- | ------------ |
| INNER JOIN | Returns matching rows | `FROM table1 INNER JOIN table2 ON condition` |
| LEFT JOIN | Returns all rows from left table | `FROM table1 LEFT JOIN table2 ON condition` |
| RIGHT JOIN | Returns all rows from right table | `FROM table1 RIGHT JOIN table2 ON condition` |
| FULL JOIN | Returns rows when there's a match in either table | `FROM table1 FULL JOIN table2 ON condition` |
| CROSS JOIN | Returns Cartesian product | `FROM table1 CROSS JOIN table2` |

### Functions

| Function Type | Examples | Basic Syntax |
| ------------- | -------- | ------------ |
| Aggregate | COUNT, SUM, AVG, MIN, MAX | `SELECT COUNT(column) FROM table` |
| String | CONCAT, UPPER, LOWER, LENGTH, SUBSTRING | `SELECT UPPER(column) FROM table` |
| Date/Time | NOW, CURRENT_DATE, DATE_PART, EXTRACT | `SELECT NOW()` |
| Conversion | CAST, CONVERT | `SELECT CAST(column AS type)` |
| Conditional | CASE, COALESCE, NULLIF | `SELECT CASE WHEN ... THEN ... END` |

### Constraints

| Constraint | Purpose | Basic Syntax (in CREATE TABLE) |
| ---------- | ------- | ------------------------------ |
| PRIMARY KEY | Uniquely identifies each record | `column_name data_type PRIMARY KEY` |
| FOREIGN KEY | Links to another table | `FOREIGN KEY (column) REFERENCES table(column)` |
| UNIQUE | Ensures unique values | `column_name data_type UNIQUE` |
| NOT NULL | Ensures column cannot be NULL | `column_name data_type NOT NULL` |
| CHECK | Validates data based on condition | `CHECK (condition)` |
| DEFAULT | Sets default value | `column_name data_type DEFAULT value` |

### Transactions

| Command | Purpose | Basic Syntax |
| ------- | ------- | ------------ |
| BEGIN | Starts a transaction | `BEGIN;` |
| COMMIT | Saves transaction changes | `COMMIT;` |
| ROLLBACK | Reverts transaction changes | `ROLLBACK;` |
| SAVEPOINT | Creates a point to roll back to | `SAVEPOINT name;` |
| ROLLBACK TO | Rolls back to a savepoint | `ROLLBACK TO name;` |

## Part 2: Zoo Database Examples

### Zoo Database Schema

````

### Advanced Joins and Set Operations

#### Different Join Types

```sql
-- Inner join: animals and their health records
SELECT a.name, hr.check_date, hr.diagnosis
FROM animals a
INNER JOIN health_records hr ON a.id = hr.animal_id;

-- Left join: all animals and their health records (if any)
SELECT a.name, hr.check_date, hr.diagnosis
FROM animals a
LEFT JOIN health_records hr ON a.id = hr.animal_id;

-- Right join: all health records and their animals
SELECT a.name, hr.check_date, hr.diagnosis
FROM animals a
RIGHT JOIN health_records hr ON a.id = hr.animal_id;

-- Full join: all animals and all health records
SELECT a.name, hr.check_date, hr.diagnosis
FROM animals a
FULL JOIN health_records hr ON a.id = hr.animal_id;

-- Multi-table join
SELECT a.name, fs.feed_time, fs.feed_type, zk.first_name, zk.last_name
FROM animals a
JOIN feeding_schedule fs ON a.id = fs.animal_id
JOIN species s ON a.species_id = s.id
JOIN habitats h ON s.habitat_id = h.id
JOIN assignments asn ON h.id = asn.habitat_id
JOIN zookeepers zk ON asn.zookeeper_id = zk.id
WHERE asn.role = 'Primary Caretaker' AND asn.end_date IS NULL;
```

#### Set Operations

```sql
-- Create a new table for visiting schools
CREATE TABLE school_visits (
  id SERIAL PRIMARY KEY,
  school_name VARCHAR(100) NOT NULL,
  visit_date DATE NOT NULL,
  student_count INTEGER NOT NULL,
  teacher_count INTEGER NOT NULL
);

INSERT INTO school_visits (school_name, visit_date, student_count, teacher_count)
VALUES
  ('Lincoln Elementary', '2023-05-10', 45, 3),
  ('Washington High', '2023-05-12', 30, 2),
  ('Jefferson Middle', '2023-05-15', 50, 4);

-- UNION: Combine regular visitors and school visits
SELECT visit_date, 'Regular' AS visit_type, adult_count + child_count AS total_visitors
FROM visitors
WHERE visit_date >= '2023-05-01'
UNION
SELECT visit_date, 'School', student_count + teacher_count
FROM school_visits
ORDER BY visit_date;

-- INTERSECT: Find dates that had both regular visitors and school visits
SELECT visit_date FROM visitors
INTERSECT
SELECT visit_date FROM school_visits;

-- EXCEPT: Find dates that had regular visitors but no school visits
SELECT visit_date FROM visitors
EXCEPT
SELECT visit_date FROM school_visits;
```

### Transaction Examples

```sql
-- Start a transaction
BEGIN;

-- Transfer an animal between habitats (requires multiple updates)
UPDATE species SET habitat_id = 3 WHERE id = 4; -- Move Meerkats to Rainforest

-- Update assignment to reflect the change
INSERT INTO assignments (zookeeper_id, habitat_id, assigned_date, role)
VALUES (2, 4, CURRENT_DATE, 'Temporary Caretaker');

-- If everything looks good, commit the changes
COMMIT;

-- Or if there's a problem, roll back
-- ROLLBACK;

-- Transaction with savepoints
BEGIN;

-- Update weights in health records
UPDATE health_records SET weight = weight * 1.05 WHERE animal_id IN (1, 2, 3);
SAVEPOINT weight_updates;

-- Change some diagnoses
UPDATE health_records SET diagnosis = 'Excellent health' WHERE diagnosis = 'Healthy';
SAVEPOINT diagnosis_updates;

-- Oops, don't want the diagnosis changes
ROLLBACK TO diagnosis_updates;

-- Commit the weight updates only
COMMIT;
```

### Analyzing and Optimizing Queries

```sql
-- Analyze query performance
EXPLAIN ANALYZE
SELECT a.name, s.name AS species_name, h.name AS habitat_name
FROM animals a
JOIN species s ON a.species_id = s.id
JOIN habitats h ON s.habitat_id = h.id
WHERE h.climate = 'Tropical';

-- Example of optimizing a slow query

-- Original query (potentially slow with large tables)
EXPLAIN ANALYZE
SELECT a.name, COUNT(hr.id) AS check_count
FROM animals a
LEFT JOIN health_records hr ON a.id = hr.animal_id
WHERE a.acquisition_date > '2019-01-01'
GROUP BY a.id, a.name
ORDER BY check_count DESC;

-- Optimized query using a subquery to reduce the dataset early
EXPLAIN ANALYZE
WITH recent_animals AS (
  SELECT id, name FROM animals WHERE acquisition_date > '2019-01-01'
)
SELECT ra.name, COUNT(hr.id) AS check_count
FROM recent_animals ra
LEFT JOIN health_records hr ON ra.id = hr.animal_id
GROUP BY ra.id, ra.name
ORDER BY check_count DESC;
```

### Importing and Exporting Data

```sql
-- Export data to CSV
\copy (SELECT * FROM animals) TO '/tmp/animals_export.csv' WITH (FORMAT csv, HEADER true);

-- Export specific columns or query results
\copy (SELECT name, species, gender, birth_date FROM animals WHERE status = 'Active') TO '/tmp/active_animals.csv' WITH (FORMAT csv, HEADER true);

-- Import data from CSV
\copy animals(name, species, birth_date, gender, acquisition_date, status, species_id) FROM '/tmp/new_animals.csv' WITH (FORMAT csv, HEADER true);
```

### Database Administration Tasks

````sql
-- Create a new user
CREATE USER zoo_viewer WITH PASSWORD 'secure_password';

-- Grant read-only privileges
GRANT SELECT ON ALL TABLES IN SCHEMA public TO zoo_viewer;

-- Create a backup of the database
-- (Run from terminal, not psql)
-- pg_dump zoo > zoo_backup.sql

-- Restore a database from backup
-- (Run from terminal, not psql)
-- psql -d zoo -f zoo_backup.sql

-- View database size
SELECT pg_size_pretty(pg_database_size('zoo'));

-- View table sizes
SELECT 
  table_name, 
  pg_size_pretty(pg_total_relation_size(table_name)) AS total_size
FROM information_schema.tables
WHERE table_schema = 'public'
ORDER BY pg_total_relation_size(table_name) DESC;
```sql
-- Create the database
CREATE DATABASE zoo;

-- Connect to the database
\c zoo

-- Create animals table
CREATE TABLE animals (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  species VARCHAR(100) NOT NULL,
  birth_date DATE,
  gender CHAR(1) CHECK (gender IN ('M', 'F')),
  acquisition_date DATE NOT NULL,
  status VARCHAR(20) DEFAULT 'Active' CHECK (status IN ('Active', 'Deceased', 'Transferred')),
  species_id INTEGER
);

-- Create species table
CREATE TABLE species (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL UNIQUE,
  scientific_name VARCHAR(100) NOT NULL,
  conservation_status VARCHAR(20) CHECK (conservation_status IN 
    ('Extinct', 'Extinct in Wild', 'Critically Endangered', 'Endangered', 
     'Vulnerable', 'Near Threatened', 'Least Concern', 'Data Deficient')),
  habitat_id INTEGER
);

-- Create habitats table
CREATE TABLE habitats (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL UNIQUE,
  climate VARCHAR(50) NOT NULL,
  area_sqm NUMERIC(10, 2) NOT NULL,
  max_capacity INTEGER,
  description TEXT
);

-- Create zookeepers table
CREATE TABLE zookeepers (
  id SERIAL PRIMARY KEY,
  first_name VARCHAR(50) NOT NULL,
  last_name VARCHAR(50) NOT NULL,
  hire_date DATE NOT NULL,
  specialization VARCHAR(100),
  contact_number VARCHAR(20)
);

-- Create assignments table (junction table for zookeepers and habitats)
CREATE TABLE assignments (
  zookeeper_id INTEGER REFERENCES zookeepers(id) ON DELETE CASCADE,
  habitat_id INTEGER REFERENCES habitats(id) ON DELETE CASCADE,
  assigned_date DATE NOT NULL,
  end_date DATE,
  role VARCHAR(50) NOT NULL,
  PRIMARY KEY (zookeeper_id, habitat_id, assigned_date)
);

-- Create feeding_schedule table
CREATE TABLE feeding_schedule (
  id SERIAL PRIMARY KEY,
  animal_id INTEGER REFERENCES animals(id) ON DELETE CASCADE,
  feed_time TIME NOT NULL,
  feed_type VARCHAR(100) NOT NULL,
  amount NUMERIC(5, 2) NOT NULL,
  notes TEXT
);

-- Create health_records table
CREATE TABLE health_records (
  id SERIAL PRIMARY KEY,
  animal_id INTEGER REFERENCES animals(id) ON DELETE CASCADE,
  check_date DATE NOT NULL,
  weight NUMERIC(6, 2),
  vet_name VARCHAR(100) NOT NULL,
  diagnosis TEXT,
  treatment TEXT
);

-- Add foreign keys after all tables are created
ALTER TABLE animals ADD FOREIGN KEY (species_id) REFERENCES species(id);
ALTER TABLE species ADD FOREIGN KEY (habitat_id) REFERENCES habitats(id);

-- Add indexes for frequently queried columns
CREATE INDEX idx_animals_species_id ON animals(species_id);
CREATE INDEX idx_species_habitat_id ON species(habitat_id);
CREATE INDEX idx_feeding_animal_id ON feeding_schedule(animal_id);
CREATE INDEX idx_health_animal_id ON health_records(animal_id);
````

### Sample Data Insertion

```sql
-- Insert habitats
INSERT INTO habitats (name, climate, area_sqm, max_capacity, description)
VALUES
  ('African Savanna', 'Tropical', 5000.00, 20, 'Warm grassland with scattered trees'),
  ('Arctic Tundra', 'Polar', 3000.00, 15, 'Cold environment with snow and ice'),
  ('Rainforest', 'Tropical', 4000.00, 25, 'Dense forest with high rainfall'),
  ('Desert', 'Arid', 2500.00, 10, 'Hot and dry environment with minimal vegetation'),
  ('Ocean Tank', 'Marine', 10000.00, 50, 'Large saltwater aquarium for marine life');

-- Insert species
INSERT INTO species (name, scientific_name, conservation_status, habitat_id)
VALUES
  ('Lion', 'Panthera leo', 'Vulnerable', 1),
  ('Polar Bear', 'Ursus maritimus', 'Vulnerable', 2),
  ('Gorilla', 'Gorilla gorilla', 'Critically Endangered', 3),
  ('Meerkat', 'Suricata suricatta', 'Least Concern', 4),
  ('Dolphin', 'Tursiops truncatus', 'Least Concern', 5),
  ('Elephant', 'Loxodonta africana', 'Endangered', 1),
  ('Penguin', 'Spheniscus demersus', 'Endangered', 2),
  ('Toucan', 'Ramphastos toco', 'Least Concern', 3);

-- Insert animals
INSERT INTO animals (name, species, birth_date, gender, acquisition_date, status, species_id)
VALUES
  ('Simba', 'Lion', '2018-06-15', 'M', '2019-01-10', 'Active', 1),
  ('Nala', 'Lion', '2019-03-22', 'F', '2019-10-05', 'Active', 1),
  ('Frost', 'Polar Bear', '2017-12-03', 'M', '2018-05-20', 'Active', 2),
  ('Luna', 'Polar Bear', '2016-02-10', 'F', '2017-11-15', 'Active', 2),
  ('Kong', 'Gorilla', '2015-07-18', 'M', '2016-08-30', 'Active', 3),
  ('Timon', 'Meerkat', '2020-01-05', 'M', '2020-03-10', 'Active', 4),
  ('Splash', 'Dolphin', '2019-09-12', 'F', '2020-06-22', 'Active', 5),
  ('Dumbo', 'Elephant', '2016-05-08', 'M', '2017-07-15', 'Active', 6),
  ('Pingu', 'Penguin', '2019-11-20', 'M', '2020-02-10', 'Active', 7),
  ('Rico', 'Toucan', '2020-04-11', 'M', '2020-08-05', 'Active', 8);

-- Insert zookeepers
INSERT INTO zookeepers (first_name, last_name, hire_date, specialization, contact_number)
VALUES
  ('John', 'Smith', '2015-03-15', 'Large Mammals', '555-1234'),
  ('Sarah', 'Johnson', '2017-06-22', 'Birds', '555-2345'),
  ('Michael', 'Williams', '2016-09-10', 'Reptiles', '555-3456'),
  ('Emily', 'Brown', '2018-01-05', 'Marine Life', '555-4567'),
  ('David', 'Jones', '2019-04-18', 'Primates', '555-5678');

-- Insert assignments
INSERT INTO assignments (zookeeper_id, habitat_id, assigned_date, end_date, role)
VALUES
  (1, 1, '2020-01-01', NULL, 'Primary Caretaker'),
  (1, 2, '2020-01-01', NULL, 'Secondary Caretaker'),
  (2, 3, '2020-01-01', NULL, 'Primary Caretaker'),
  (3, 4, '2020-01-01', NULL, 'Primary Caretaker'),
  (4, 5, '2020-01-01', NULL, 'Primary Caretaker'),
  (5, 3, '2020-01-01', NULL, 'Secondary Caretaker');

-- Insert feeding schedules
INSERT INTO feeding_schedule (animal_id, feed_time, feed_type, amount, notes)
VALUES
  (1, '08:00:00', 'Meat', 5.5, 'Large portion of beef'),
  (1, '16:00:00', 'Meat', 4.5, 'Mixed meats'),
  (2, '08:30:00', 'Meat', 4.0, 'Chicken and beef mix'),
  (3, '09:00:00', 'Fish', 6.0, 'Salmon and other fish'),
  (5, '10:00:00', 'Fruits', 3.0, 'Assorted fruits and vegetables'),
  (8, '07:30:00', 'Plants', 20.0, 'Hay and vegetables'),
  (9, '11:00:00', 'Fish', 1.5, 'Small fish');

-- Insert health records
INSERT INTO health_records (animal_id, check_date, weight, vet_name, diagnosis, treatment)
VALUES
  (1, '2023-01-15', 180.5, 'Dr. Martinez', 'Healthy', 'Routine checkup'),
  (2, '2023-01-20', 125.3, 'Dr. Martinez', 'Mild infection', 'Prescribed antibiotics'),
  (3, '2023-02-05', 450.0, 'Dr. Wilson', 'Minor injury', 'Wound cleaning and bandage'),
  (5, '2023-02-12', 195.5, 'Dr. Wilson', 'Healthy', 'Routine checkup'),
  (8, '2023-03-01', 2500.0, 'Dr. Martinez', 'Healthy', 'Tusk examination and routine checkup');
```

### DDL Examples

#### Creating a New Table

```sql
-- Create a new table for visitors
CREATE TABLE visitors (
  id SERIAL PRIMARY KEY,
  visit_date DATE NOT NULL,
  adult_count INTEGER NOT NULL,
  child_count INTEGER NOT NULL,
  total_revenue DECIMAL(10, 2) NOT NULL,
  special_event BOOLEAN DEFAULT false,
  notes TEXT
);
```

#### Altering a Table

```sql
-- Add a column to the animals table
ALTER TABLE animals ADD COLUMN is_exhibit BOOLEAN DEFAULT true;

-- Change data type of a column
ALTER TABLE visitors ALTER COLUMN notes TYPE VARCHAR(500);

-- Add a check constraint
ALTER TABLE visitors ADD CONSTRAINT positive_counts 
  CHECK (adult_count >= 0 AND child_count >= 0);
```

#### Creating and Dropping Indexes

```sql
-- Create an index on visit_date for faster searching
CREATE INDEX idx_visit_date ON visitors(visit_date);

-- Drop an index
DROP INDEX idx_visit_date;
```

#### Creating a View

```sql
-- Create a view for active animals with their species and habitat information
CREATE VIEW animal_habitat_view AS
SELECT 
  a.id, a.name, a.gender, a.status,
  s.name AS species_name, s.conservation_status,
  h.name AS habitat_name, h.climate
FROM animals a
JOIN species s ON a.species_id = s.id
JOIN habitats h ON s.habitat_id = h.id
WHERE a.status = 'Active';
```

### DML Examples

#### SELECT Queries

```sql
-- Basic SELECT
SELECT * FROM animals;

-- SELECT with column selection
SELECT name, species, gender FROM animals;

-- SELECT with WHERE clause
SELECT name, species FROM animals WHERE status = 'Active' AND gender = 'F';

-- SELECT with ORDER BY
SELECT name, birth_date FROM animals ORDER BY birth_date DESC;

-- SELECT with LIMIT
SELECT * FROM animals LIMIT 5;

-- SELECT with GROUP BY and aggregate functions
SELECT species, COUNT(*) AS animal_count 
FROM animals 
GROUP BY species;

-- SELECT with HAVING
SELECT species, COUNT(*) AS animal_count 
FROM animals 
GROUP BY species 
HAVING COUNT(*) > 1;

-- SELECT with JOIN
SELECT a.name, s.name AS species_name, h.name AS habitat_name
FROM animals a
JOIN species s ON a.species_id = s.id
JOIN habitats h ON s.habitat_id = h.id;
```

#### INSERT Examples

```sql
-- Insert a single record
INSERT INTO health_records (animal_id, check_date, weight, vet_name, diagnosis, treatment)
VALUES (4, CURRENT_DATE, 320.5, 'Dr. Wilson', 'Healthy', 'Annual checkup');

-- Insert multiple records
INSERT INTO feeding_schedule (animal_id, feed_time, feed_type, amount, notes)
VALUES
  (4, '08:00:00', 'Fish', 5.0, 'Fresh fish only'),
  (6, '09:30:00', 'Insects', 0.5, 'Mealworms and crickets'),
  (7, '10:15:00', 'Fish', 7.0, 'Various fish types');
```

#### UPDATE Examples

```sql
-- Update a single record
UPDATE animals SET status = 'Transferred' WHERE id = 6;

-- Update multiple records
UPDATE animals 
SET acquisition_date = '2021-01-01' 
WHERE acquisition_date < '2019-01-01';

-- Update with calculation
UPDATE feeding_schedule 
SET amount = amount * 1.1 
WHERE feed_type = 'Meat';
```

#### DELETE Examples

```sql
-- Delete specific records
DELETE FROM health_records WHERE check_date < '2022-01-01';

-- Delete with multiple conditions
DELETE FROM feeding_schedule 
WHERE animal_id = 3 AND feed_time < '09:00:00';
```

### Advanced Query Examples

#### Subqueries

```sql
-- Subquery in WHERE clause
SELECT name 
FROM animals 
WHERE species_id IN (
  SELECT id FROM species WHERE conservation_status = 'Endangered'
);

-- Subquery in FROM clause
SELECT avg_weight, species_name
FROM (
  SELECT s.name AS species_name, AVG(hr.weight) AS avg_weight
  FROM health_records hr
  JOIN animals a ON hr.animal_id = a.id
  JOIN species s ON a.species_id = s.id
  GROUP BY s.name
) AS weight_stats
WHERE avg_weight > 100;

-- Correlated subquery
SELECT a.name, a.species
FROM animals a
WHERE a.id IN (
  SELECT hr.animal_id
  FROM health_records hr
  WHERE hr.animal_id = a.id AND hr.diagnosis LIKE '%injury%'
);
```

#### Using EXISTS

```sql
-- Find animals that have health records
SELECT a.name
FROM animals a
WHERE EXISTS (
  SELECT 1 FROM health_records hr WHERE hr.animal_id = a.id
);

-- Find animals without feeding schedules
SELECT a.name
FROM animals a
WHERE NOT EXISTS (
  SELECT 1 FROM feeding_schedule fs WHERE fs.animal_id = a.id
);
```

#### Using CASE

```sql
-- Categorize animals by age
SELECT 
  name,
  birth_date,
  CASE
    WHEN birth_date > '2020-01-01' THEN 'Young'
    WHEN birth_date > '2018-01-01' THEN 'Adult'
    ELSE 'Senior'
  END AS age_category
FROM animals;

-- Classify habitats by size
SELECT 
  name,
  area_sqm,
  CASE
    WHEN area_sqm < 3000 THEN 'Small'
    WHEN area_sqm < 5000 THEN 'Medium'
    ELSE 'Large'
  END AS size_classification
FROM habitats;
```

#### Window Functions

```sql
-- Rank animals by weight within species
SELECT 
  a.name,
  s.name AS species,
  hr.weight,
  RANK() OVER (PARTITION BY a.species_id ORDER BY hr.weight DESC) AS weight_rank
FROM animals a
JOIN health_records hr ON a.id = hr.animal_id
JOIN species s ON a.species_id = s.id;

-- Calculate running totals
SELECT 
  visit_date,
  adult_count + child_count AS total_visitors,
  SUM(adult_count + child_count) OVER (ORDER BY visit_date) AS running_total
FROM visitors;

-- Calculate moving averages
SELECT 
  visit_date,
  total_revenue,
  AVG(total_revenue) OVER (
    ORDER BY visit_date 
    ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
  ) AS weekly_moving_avg
FROM visitors;

-- Row numbers, lead and lag
SELECT 
  a.name,
  hr.check_date,
  hr.weight,
  LAG(hr.weight) OVER (PARTITION BY a.id ORDER BY hr.check_date) AS previous_weight,
  hr.weight - LAG(hr.weight) OVER (PARTITION BY a.id ORDER BY hr.check_date) AS weight_change
FROM animals a
JOIN health_records hr ON a.id = hr.animal_id
ORDER BY a.id, hr.check_date;
```

#### Common Table Expressions (CTE)

```sql
-- CTE for animals and their habitats
WITH animal_habitats AS (
  SELECT a.id, a.name, s.name AS species_name, h.name AS habitat_name
  FROM animals a
  JOIN species s ON a.species_id = s.id
  JOIN habitats h ON s.habitat_id = h.id
)
SELECT * FROM animal_habitats WHERE habitat_name = 'African Savanna';

-- Multiple CTEs
WITH endangered_species AS (
  SELECT id, name 
  FROM species 
  WHERE conservation_status IN ('Endangered', 'Critically Endangered')
),
endangered_animals AS (
  SELECT a.id, a.name, a.gender, es.name AS species_name
  FROM animals a
  JOIN endangered_species es ON a.species_id = es.id
)
SELECT * FROM endangered_animals ORDER BY species_name, name;

-- Recursive CTE for organizational hierarchy (if we add a manager_id to zookeepers)
ALTER TABLE zookeepers ADD COLUMN manager_id INTEGER REFERENCES zookeepers(id);
UPDATE zookeepers SET manager_id = 1 WHERE id IN (2, 3);
UPDATE zookeepers SET manager_id = 2 WHERE id IN (4, 5);

WITH RECURSIVE org_hierarchy AS (
  -- Base case: top-level employee with no manager
  SELECT id, first_name, last_name, manager_id, 1 AS level
  FROM zookeepers
  WHERE manager_id IS NULL
  
  UNION ALL
  
  -- Recursive case: employees with managers
  SELECT z.id, z.first_name, z.last_name, z.manager_id, oh.level + 1
  FROM zookeepers z
  JOIN org_hierarchy oh ON z.manager_id = oh.id
)
SELECT id, first_name, last_name, level
FROM org_hierarchy
ORDER BY level, id;
```