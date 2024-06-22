### SQL Basics

**SQL (Structured Query Language)** is a standard programming language used to manage and manipulate relational databases. Here's an overview of some basic SQL commands:

1. **SELECT**: Retrieves data from a database.
2. **INSERT**: Adds new rows to a table.
3. **UPDATE**: Modifies existing data within a table.
4. **DELETE**: Removes data from a table.
5. **CREATE TABLE**: Creates a new table.
6. **DROP TABLE**: Deletes a table.
7. **ALTER TABLE**: Modifies an existing table (e.g., adding a column).
8. **JOIN**: Combines rows from two or more tables based on a related column.

### Basic SQL Commands with Examples

Let's start with simple SQL commands. We'll use a hypothetical database with a single table named `employees`.

#### CREATE TABLE
The CREATE TABLE statement is used to create a new table in the database. You define the table's name and its columns, specifying the data type for each column.

Syntax:
```sql
CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    ...
);
```

Example:
```sql
CREATE TABLE employees (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    age INTEGER,
    department TEXT
);
```
- `id` INTEGER PRIMARY KEY: Defines an id column as an integer and sets it as the primary key, ensuring uniqueness.
- `name` TEXT NOT NULL: Defines a name column as text and specifies that it cannot be null.
- `age` INTEGER: Defines an age column as an integer.
- `department` TEXT: Defines a department column as text.

#### INSERT
The INSERT statement adds new rows to a table.

Syntax:
```sql
INSERT INTO table_name (column1, column2, ...) VALUES (value1, value2, ...);
```

Example:
```sql
INSERT INTO employees (name, age, department) VALUES ('Alice', 30, 'HR');
INSERT INTO employees (name, age, department) VALUES ('Bob', 25, 'Engineering');
Inserts two new rows into the employees table with specified values for each column.
```

#### SELECT
The SELECT statement retrieves data from a database. You can specify which columns to retrieve and filter the results using WHERE.

Syntax:
```sql
SELECT column1, column2, ... FROM table_name WHERE condition;
```

Examples:
```sql
SELECT * FROM employees;
```
- Retrieves all columns and rows from the employees table.

```sql
SELECT name, age FROM employees WHERE department = 'HR';
```
- Retrieves the name and age columns for rows where the department is 'HR'.
  
#### UPDATE
The UPDATE statement modifies existing data in a table. You can specify which rows to update using the WHERE clause.

Syntax:
```sql
UPDATE table_name SET column1 = value1, column2 = value2, ... WHERE condition;
```

Example:
```sql
UPDATE employees SET age = 31 WHERE name = 'Alice';
```
- Updates the age of the employee named 'Alice' to 31.

#### DELETE
The DELETE statement removes rows from a table. You can specify which rows to delete using the WHERE clause.

Syntax:
```sql
DELETE FROM table_name WHERE condition;
```

Example:
```sql
DELETE FROM employees WHERE name = 'Bob';
```
- Deletes the row from the employees table where the name is 'Bob'.
  
#### ALTER TABLE
The ALTER TABLE statement modifies an existing table structure, such as adding or removing columns.

Syntax:
```sql
ALTER TABLE table_name ADD column_name datatype;
ALTER TABLE table_name DROP COLUMN column_name;
```

Example:
```sql
ALTER TABLE employees ADD salary INTEGER;
```
- Adds a new column salary of type integer to the employees table.
  
#### DROP TABLE
The DROP TABLE statement deletes a table and all its data from the database.

Syntax:
```sql
DROP TABLE table_name;
```

Example:
```sql
DROP TABLE employees;
```
- Deletes the employees table and all its data.

#### JOIN
The JOIN clause is used to combine rows from two or more tables based on a related column between them. There are several types of joins: INNER JOIN, LEFT JOIN, RIGHT JOIN, and FULL OUTER JOIN.

Syntax:
```sql
SELECT columns FROM table1
JOIN_TYPE table2
ON table1.common_column = table2.common_column;
```

Example:
Assuming there are two tables, employees and departments:
```sql
CREATE TABLE departments (
    department_id INTEGER PRIMARY KEY,
    department_name TEXT NOT NULL
);

INSERT INTO departments (department_id, department_name) VALUES (1, 'HR');
INSERT INTO departments (department_id, department_name) VALUES (2, 'Engineering');

SELECT employees.name, departments.department_name
FROM employees
INNER JOIN departments ON employees.department = departments.department_name;
Combines rows from employees and departments where the department column in employees matches the department_name column in departments.
```

### Using SQLite with Python

SQLite is a lightweight, disk-based database that doesn't require a separate server process. Python's `sqlite3` module provides a way to interact with SQLite databases.

Here's a step-by-step guide to using SQLite with Python.

#### Step 1: Importing SQLite3 and Connecting to a Database

```python
import sqlite3

# Connect to a database (or create one if it doesn't exist)
conn = sqlite3.connect('example.db')

# Create a cursor object
cur = conn.cursor()
```

#### Step 2: Creating a Table

```python
cur.execute('''
    CREATE TABLE IF NOT EXISTS employees (
        id INTEGER PRIMARY KEY,
        name TEXT NOT NULL,
        age INTEGER,
        department TEXT
    )
''')

# Commit the changes
conn.commit()
```

#### Step 3: Inserting Data

```python
cur.execute("INSERT INTO employees (name, age, department) VALUES (?, ?, ?)", ('Alice', 30, 'HR'))
cur.execute("INSERT INTO employees (name, age, department) VALUES (?, ?, ?)", ('Bob', 25, 'Engineering'))

# Commit the changes
conn.commit()
```

#### Step 4: Querying Data

```python
cur.execute("SELECT * FROM employees")
rows = cur.fetchall()
for row in rows:
    print(row)
```

#### Step 5: Updating Data

```python
cur.execute("UPDATE employees SET age = ? WHERE name = ?", (31, 'Alice'))

# Commit the changes
conn.commit()
```

#### Step 6: Deleting Data

```python
cur.execute("DELETE FROM employees WHERE name = ?", ('Bob',))

# Commit the changes
conn.commit()
```

#### Step 7: Closing the Connection

```python
# Close the cursor and connection
cur.close()
conn.close()
```

### Complete Example in Python

Here's a complete Python script that includes all the steps mentioned above:

```python
import sqlite3

# Connect to the database
conn = sqlite3.connect('example.db')
cur = conn.cursor()

# Create table
cur.execute('''
    CREATE TABLE IF NOT EXISTS employees (
        id INTEGER PRIMARY KEY,
        name TEXT NOT NULL,
        age INTEGER,
        department TEXT
    )
''')

# Insert data
cur.execute("INSERT INTO employees (name, age, department) VALUES (?, ?, ?)", ('Alice', 30, 'HR'))
cur.execute("INSERT INTO employees (name, age, department) VALUES (?, ?, ?)", ('Bob', 25, 'Engineering'))

# Commit changes
conn.commit()

# Query data
cur.execute("SELECT * FROM employees")
rows = cur.fetchall()
for row in rows:
    print(row)

# Update data
cur.execute("UPDATE employees SET age = ? WHERE name = ?", (31, 'Alice'))
conn.commit()

# Delete data
cur.execute("DELETE FROM employees WHERE name = ?", ('Bob',))
conn.commit()

# Close the connection
cur.close()
conn.close()
```
