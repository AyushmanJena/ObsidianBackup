Creating database 
```sql
CREATE DATABASE databaseName;
CREATE DATABASE school;

CREATE DATABASE IF NOT EXISTS databaseName;
```


Deleting entire database, including all tables, data and other object within
DROP is a DDL command
```sql
DROP DATABASE databaseName;

DROP DATABASE IF EXISTS databaseName;
```

  Using a database : specify which database we want to use
```sql
USE databaseName;

USE college;  
 ```

show all databases in our server
```
SHOW DATABASES;
```

show all tables present in our current database 
```
SHOW TABLES;
```

# Table Operations 
Creating the table DDL command
```sql
CREATE TABLE TableName (
	ColumnName1 DataType1 Constraint1,
	ColumnName2 DataType2 Constraint2,
	...
);

CREATE TABLE employee(
	empId INT PRIMARY KEY,
	name VARCHAR(50),
	salary INT
);
```

constraints like : NOT NULL, PRIMARY KEY, etc.

Inserting Data into table : DML command
```sql
INSERT INTO tableName(column1, column2, ...) VALUES (value1, value2, ...);

INSERT INTO employee(empId, name, salary) VALUES
(1, "Raj", 12300), (2, "Prem", 12300), (3, "Lakhan", 12300);
```


Viewing values inserted into our table
```sql
-- See specific values of a column : 
SELECT empId FROM employee;

-- See all the values or the entire table :
SELECT * FROM employee;
```


# Datatypes in SQL : 

Numeric : 
- INTEGER/INT  
- SMALLINT
- BIGINT
- DECIMAL(p,s) : provides exact precision p is precision and s is scale
- FLOAT
- DOUBLE

Character / String : 
- CHAR(n)
- VARCHAR(n) : less memory wasted as compared to CHAR
- TEXT : no specified length

Date and Time : 
- DATE : YYYY-MM-DD
- TIME : hh:mm:ss
- DATETIME : yyyy-mm-dd hh:mm:ss
- TIMESTAMP

Boolean: 
- BOOLEAN

Binary : 
- BINARY(n) : fixed length binary data
- VARBINARY(n) : variable length binary data
- BLOB : Binary Large Object


UNSIGNED : By default all numeric datatypes can have negative as well as positive values. This restrict the range, so if we know there is only +ve values which is stored, we use UNSIGNED attribute(0-255)
```
CREATE TABLE student(
	id INT UNSIGNED
);
```

# KEYS
**Primary Key** : A primary key is a unique identifier for each record in the table. 
It ensures that each row can be uniquely identified and accessed within the table.
Primary Key is always unique and not null.
Ex : id, aadhar number

**Foreign Key** : A foreign key is a field in a table that refers  to the primary key in another table. 
It established relationships between tables.

```
student :
roll | name | hometown
1      a       x
2      b       y
3      c       z
(for student table roll is the primary key)

subject : 
roll | name | subject
1       a      p
2       b      q
3       c      r
(for subject table roll is the foreign key)
```

the roll number attribute of subject has same value as the roll in the student table, so it would reference or take values from the student table

Referenced table : Table having primary key (also called base table or parent table)
Referencing table : Table having foreign key (also called child table)


# Constraints
Constraints define rules or conditions that must be satisfied by the data in the table.
Common constraints include uniqueness, nullability, default values, etc.


`UNIQUE`: Ensures values in a column are unique across the table
`NOT NULL` : Ensures a column cannot have a null value
`CHECK` : Enforces a condition to be true for each row
`DEFAULT` : Provides a default value for a column if no value is specified
`PRIMARY KEY` : Enforces the uniqueness of values in one or more columns
`FOREIGN KEY` : Enforces a link between two tables by referencing a column in a table that is a primary key in another table.

```sql
CREATE TABLE student(
	phone INT UNIQUE,
	id INT NOT NULL,
	age INT CHECK(age>18),
	schoolName VARCHAR(50) DEFAULT 'govt school',
	rollNumber INT PRIMARY KEY
);
```

 Note : One table can have only one primary key. But can have multiple foreign keys

Foreign key helps us to perform operations related to the parent table, such as joining tables or ensuring referential integrity.

Referential Integrity : Maintaining data consistency, if data in one table is changed, related tables should also reflect the changes as required.

Query : 
```sql
CREATE TABLE childTableName(
	childId INT PRIMARY KEY,
	baseId INT,
	FOREIGN KEY(baseId) REFERENCES parentTableName(baseId)
);

```

baseId -> PK of parentTable


# Cascading in Foreign Key
Cascading are a set of rules which dictate what actions should be taken automatically when a referenced row in the parent table is modified or deleted.

**CASCADE** : If a row in the aren't table is updated or deleted, all related rows in the child table will be automatically updated or deleted.
**SET NULL** : If a row in the parent table is updated or deleted, all corresponding foreign key values in the child table will be set to NULL.
**RESTRICT or NO ACTION** : Blocks the modification or deletion of the referenced row in the parent table if related rows exist in the child table and thus maintaining referential integrity.

## Cascading in Foreign Key
1. ON DELETE CASCADE
- The ON DELETE CASCADE clause indicates that if a row in the parent table(parent_table) is deleted, all corresponding rows in the child table(child_table) will be automatically deleted.

2. ON UPDATE CASCADE
- The ON UPDATE CASCADE clause indicates that if a row in the parent table (parent_table) is updated, all corresponding rows in the child table(child_table) will be automatically updated as well.
```sql
CREATE TABLE childtableName(
	childId INT PRIMARY KEY,
	baseId INT,
	FOREIGN KEY(baseId) REFERENCES parenttableName(baseId) ON UPDATE CASCADE
);
```



# Types of SQL commands
### DQL 
Data Query Language
When we want to fetch data from our database
SELECT 

### DML
Data Manipulation Language
When we want to make changes to our data
INSERT, UPDATE, DELETE

### DDL
Data Definition Language
Deals with the definition of the data, like creating table, doing modifications to table columns, etc.
CREATE, ALTER, DROP, TRUNCATE, RENAME

### DCL
Data Control Language
Specify the controls on our data

### TCL
Transaction Control Command
Used while performing transactions


# Update Command
 The UPDATE command in SQL is used to modify existing records in a table. 
 
 If you get a safe mode error while executing queries run this query : 
 `SET SQL_SAFE_UPDATES=0;`

QUERY :
```sql
UPDATE tableName
SET columnName1= value1, columnName2 = value2 
WHERE condition;

-- example 
UPDATE employee
SET salary=50000
WHERE department="HR";
```


# DELETE Command
The DELETE command in SQL is used to remove records from a table

```sql
DELETE FROM tableName
WHERE condition;

-- example
DELETE FROM employee
WHERE department = "HR";
```


# SELECT Command
SELECT command is a DQL (Data Query Language) command. It is used to retrieve data from a database.

We can provide specific columns from which we can retrieve data.

```SQL
-- to retrieve data present in specific column in a table
SELECT column1, column2 FROM tableName;

-- to retrieve all the data present in a table
SELECT * FROM tableName;


-- examples :
SELECT name, age FROM employee;
SELECT * FROM employee;
```


# WHERE clause
It filters the rows based on specified conditions

```SQL
SELECT col1, col2 FROM tableName WHERE condition;

-- example
SELECT name, age FROM employee WHERE aga>20;
```


# Alter Command
Alter is a DDL command used to modify(change) existing database objects, such as tables, indexes or constraints(schema)

Ex: 
Adding a new column, deleting a column
Changing name of attribute from id to empId

Adding a column
```sql
ALTER TABLE tableName
ADD columnName datatype constraint;

ex : 
ALTER TABLE employee
ADD dob VARCHAR(20);
```

Drop a column
```sql
ALTER TABLE tableName
DROP COLUMN columnName;

ex : 
ALTER TABLE employee
DROP COLUMN dob;
```

Modify the datatype of an existing column
- The MODIFY clause is often used within an ALTER TABLE statement in SQL. 
- It allows us to change the definition or properties of an existing column in a table.
```sql
-- new datatype for existing column
ALTER TABLE tableName
MODIFY columnName newDataType;

-- ex:
ALTER TABLE employee
MODIFY age NUMBER;

```

Change the name of an existing column
- The CHANGE command is often used within an ALTER TABLE statement in SQL.
- It helps to change the name or data type of a column within a table
```sql
ALTER TABLE tableName
CHANGE oldColumnName newColumnName newDataType;

-- ex :
ALTER TABLE employee
CHANGE age emp_age VARCHAR(3);
```

# RENAME Command
- The RENAME command is used to change the name of an existing database object, such as a table, column, index or constraint.

Rename the name of Database : 
```sql
RENAME DATABASE DatabaseName TO newDatabaseName;
```

Rename the name of table: 
```SQL
RENAME TABLE oldTableName TO newTableName;

-- ex : 
RENAME TABLE employee TO employees;
```

Rename the name of an existing column
```sql
ALTER TABLE tableName
RENAME COLUMN oldColumnName TO newColumnName;

-- ex :
ALTER TABLE employee
RENAME COLUMN emp_age TO age;
```


# Truncate Command
- This command removes all the rows from a given table, leaving the table empty but preserving its structure.
```sql
TRUNCATE TABLE tableName;
```


## Difference between TRUNCATE, DELETE and DROP
TRUNCATE : 
- Removes all rows from a table
- `TRUNCATE TABLE tableName`

DELETE : 
- Used to remove specific rows from a table based on a condition.
- `DELETE FROM tableName WHERE condition;`

DROP : 
- Used to completely remove table
- `DROP TABLE tableName;`

# DISTINCT keyword
DISTINCT keyword is used within the SELECT statement to retrieve unique values from a column or combination of columns.

Retrieve a list of unique values from col1
```sql
SELECT DISTINCT col1 
FROM tableName;
```

Return unique combinations of col1 and col2
```sql
SELECT DISTINCT col1, col2
FROM tableName;
```
- together there should not be same combinations

# Operators in SQL
