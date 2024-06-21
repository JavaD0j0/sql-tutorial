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

#### 1. CREATE TABLE

This command creates a new table.

```sql
CREATE TABLE employees (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    age INTEGER,
    department TEXT
);
```

#### 2. INSERT

Adds new data to the table.

```sql
INSERT INTO employees (name, age, department) VALUES ('Alice', 30, 'HR');
INSERT INTO employees (name, age, department) VALUES ('Bob', 25, 'Engineering');
```

#### 3. SELECT

Retrieves data from the table.

```sql
SELECT * FROM employees;
SELECT name, age FROM employees WHERE department = 'HR';
```

#### 4. UPDATE

Modifies existing data.

```sql
UPDATE employees SET age = 31 WHERE name = 'Alice';
```

#### 5. DELETE

Removes data from the table.

```sql
DELETE FROM employees WHERE name = 'Bob';
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
