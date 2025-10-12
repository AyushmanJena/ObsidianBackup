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
To perform operations on data in SQL we use operators.

ex : 
```sql
SELECT col1 col2 FROM tableName WHERE condition(use operator);
```

### Types of operators in SQL : 
**Arithmetic Operators** : 
- addition `+`, subtraction `-`, multiplication `*`, division / , modulus `%`etc.
query : 
```sql
SELECT * FROM employee WHERE age+1=60;
```

**Comparison Operators** : 
equal to `=`
not equal to `<> or !=`
greater than `>`
greater than or equal to `>=` , etc.

query : 
```sql
SELECT * FROM employee WHERE age > 20;
```

**Logical Operator** :
AND : return true if both conditions are true
OR : return true if either conditions are true
NOT : reverses the condition result

```sql
SELECT * FROM employee WHERE city='Pune' AND age> 18;

SELECT * FROM employee WHERE city='Pune' OR age> 18;

SELECT * FROM employee WHERE department NOT IN ('IT', 'HR');
```

**IN Operator** : 
Checks if a value matches in a list of values
```sql
SELECT * FROM employee WHERE department IN ('IT', 'HR');
```

**IS NULL / IS NOT NULL Operators** : 
IS NULL (checks for null values), IS NOT NULL (checks for not null values)
```sql
SELECT * FROM employee WHERE department IS NOT NULL;

SELECT * FROM employee WHERE department IS NULL;
```

**Bitwise Operators** : 
AND(&), OR(|)   

**LIKE & Wildcard Operators :** 
LIKE operator is used to search for a specified pattern in a column.
It uses wildcard operators for matching patterns.

% (percentage sign) : It matches for any sequence of zero or more characters
```sql
SELECT * FROM employee WHERE name LIKE 'A%';
-- starts with A and can have any characters after it
```

`_` (Underscore sign) : It matches for any single character  
```sql
SELECT * FROM employee WHERE name LIKE '_A%';
-- single character before A and can have any number of char after A
```

**BETWEEN Operator** : 
Checks if a value is within a range of values
 includes both starting and ending range as well.
```sql
SELECT * FROM employee 
WHERE salary BETWEEN 1200 AND 1500;
```


# Clauses in SQL
Clauses are like tools/conditions that helps us to make queries more specific or decide what data to fetch.

Ex : WHERE, GROUP BY, HAVING, ORDER BY, LIMIT

Query : 
```
SELECT col1, col2
FROM tableName
clause condition;
```

**WHERE clause :**
Filters thr rows based on specified conditions.
```sql
SELECT col1, col2
FROM tableName
WHERE condition;

--ex : 
SELECT * FROM employee WHERE age > 20;
```

**LIMIT clause** : 
LIMIT clause is used to restrict the number of rows returned by a query.
This query retrieves the first n rows from the table.

```sql
SELECT col1, col2 FROM tableName
LIMIT noOfRows;

-- ex : 
SELECT * FROM employee LIMIT 2;
```

LIMIT n - It helps to retrieve a maximum of n rows from the beginning of the result set
LIMIT m, n - It helps to retrieve a specific range of rows where
	m : number of rows to skip from the beginning
	n : number of rows to fetch after skipping 

**ORDER BY clause** : 
It is used to sort the results in ascending or descending order. 
By default it returns the result in ascending order.

```sql
SELECT col1, col2 FROM tableName
ORDER BY col1(ASC/DESC), col2(ASC/DESC);

--ex : 
SELECT * FROM employee 
ORDER BY salary DESC;
```


# SOME QUESTIONS 
```SQL
-- write a sql query to fetch the details of employees having id as 1 and city as mumbai
SELECT * FROM employee
WHERE id=1 AND city="Mumbai";

-- Write a SQL query to fetch the details of employees having salary greater than 1200 and city as Mumbai
SELECT * FROM employee
WHERE salary>1200 AND city="Mumbai";

-- Write a SQL query to fetch the details of employees who are not from Mumbai
SELECT * FROM employee
WHERE city NOT IN("Mumbai");

-- Write a SQL query to fetch the details of employees having the maximum salary
SELECT * FROM employee
ORDER BY salary DESC;

--Write a SQL query to fetch the details of 2 employees having the maximum salary 
SELECT * FROM employee 
ORDER BY salary DESC
LIMIT 2;
```

# Aggregate Functions
Aggregate functions performs some operations on a set of rows and then returns a single value summarizing the data.
These are used with SELECT statements to perform calculations.

Types of Aggregate functions :
- COUNT()
- SUM()
- AVG()
- MIN()
- MAX()
- GROUP_CONCAT()

**COUNT() :** It counts the number of rows in a table or the number of non-null values in a column.
- This counts how many things are in a list or a group.

```sql
SELECT count(name) FROM employee; 
-- This will tell the number of employees in a company.
-- name : column name
```

**SUM() :** It calculates the sum of all values in a numeric column.
- This adds up all the numbers isn a list
```sql
SELECT SUM(salary) FROM employee; 
-- this will tell the total amount company is paying to its employees
```

**AVG() :** It computes the average of all values in a numeric column.
It finds the average or the middle number of all the numbers in a list

```sql
SELECT AVG(salary) FROM employee; 
-- this will tell the average amount of the salary
```


**MIN() :** It finds the smallest number in a list
```sql
SELECT MIN(salary) FROM employee;
-- This will tell the minimum salary company is paying to its employees
```


**MAX() :** It finds the largest number in a list
```sql
SELECT MAX(salary) FROM employee;
-- This will tell the maximum salary company is paying to its employees
```


##### Grouping data with GROUP BY clause
GROUP BY CLAUSE
This is used to group rows that have the same values together.
It helps to organise data into groups so that you can do calculations, like finding totals or averages for each group.


For example : There are various departments in a company, and you want to know the maximum salary for the IT department.
Group them and then find the max on them 


```sql
-- QUERY
SELECT col1, aggregateFun(col2) 
FROM tableName
GROUP BY col1;

--EX: 
SELECT department, AVG(salary) AS avgsal FROM employee
GROUP BY department;

-- output will be all departments and their avg salary table
```

##### HAVING clause
Having Clause : The HAVING clause is just like the where clause but the main difference is it works on aggregated data. 
It is used with the GROUP BY clause
It helps to filter groups based on given conditions.

```sql
SELECT col1, col2 aggregateFun(col3) 
FROM tableName
GROUP BY col1 col2
HAVING condition;

--EX : 
SELECT department, AVG(salary) AS avgsal
FROM employee
GROUP BY department
HAVING avgsal > 1500;
```

![[Pasted image 20250930225540.png]]
# Practice Questions 
```sql
-- Write a query to find the total number of employees in each city
SELECT city, COUNT(name) AS no_of_emp 
FROM employee
GROUP BY city;

-- Write a query to find the maximum salary of employees in each city in descending order
SELECT city, max(salary) AS max_salary
FROM employee
GROUP BY city
ORDER BY max_salary DESC;

-- Write a query to display the department names alongside the total cound of employees in each department, sorting the results by the total number of employees in descending order.
SELECT department, COUNT(id) AS totalemployees
FROM employee
GROUP BY department
ORDER BY totalemployees DESC;

-- Write a query to list the departments where the average salary is greater than 1200, also display the department name and the average salary
SELECT department, AVG(salary) AS avgsalary
FROM employee 
GROUP BY department
HAVING AVG(salary) > 50000; -- can use avgsalary as well

--
``` 


![[Pasted image 20250930232209.png]]


# Joins in SQL
Joins are used to combine rows from two or more tables based on a related or shared or common column between them.
There are commonly 4 types of joins including 
INNER JOIN
LEFT JOIN
RIGHT JOIN,
FULL JOIN,
SELF JOIN,
CROSS JOIN

Q. Is Foreign key important for performing joins?
A. Joins can be performed based on any columns that establish a relationship between tables, not just FK constraints, so its not necessary.

Note : 
- For joins if there is no value in the other table for the common field it fills the value with NULL

### Inner Join
- Get values common in both the table this join is performed on. 

![[Pasted image 20250930232932.png]]

Inner Join helps us in getting the rows that have matching values in both tables, according to the given join condition.

two tables (students, courses) have a common field (e.g. rollno)
We will get all the rows for which rollno are present in both tables

```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.colName = table2.colName;

-- ex : 
SELECT customer.id, customer.name, orders.o_name
FROM customer
INNER JOIN orders
ON customer.id = orders.id;
```

### Left Join / Left Outer Join
![[Pasted image 20251001220856.png]]

Left Join is used to fetch all the records from the left table along with matched records from the right table.
If there are no matching records in the right table, NULL values are returned for the columns of the right table.

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.colName = table2.colName;

--ex : 
SELECT *
FROM customer
LEFT JOIN orders
ON customer.id = orders.id;
```

### Right Join / Right Outer Join
We get all the values present in the right table along with values common in both the tables
![[Pasted image 20251001221026.png]]

Right Outer Join is used to fetch all the records from the right table along with matched records from the left table.
If there are no matching records in the left table, NULL values are returned for the columns of the left table.

```sql
SELECT columns 
FROM table1
RIGHT JOIN table2
ON table1.colName = table2.colName;

-- ex : 
SELECT *
FROM customer
RIGHT JOIN orders
ON customer.id = orders.id;
```

### Full Join / Full Outer Join
All the values of left and right table will be taken
![[Pasted image 20251001221202.png]]

It returns the matching rows of both left and right table and also including all rows from the both tables even if they don't have matching rows.
If there is no match, NULL values are returned for the columns of the missing table.
In MySQL, the syntax for a full join is different compared to other SQL databases like PostgreSQL or SQL server.

MySQL does not support the FULL JOIN keyword directly. So we use a command like LEFT JOIN, RIGHT JOIN AND UNION to achieve the result.
So we write the query for LEFT JOIN and query for RIGHT JOIN with a UNION between them
```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.colName = table2.colName

UNION

SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.colName = table2.colName;

```

### Self Join
A self join is a type of join where a table is joined with itself.
 ![[Pasted image 20251001221248.png]]
It is a type of inner join.
```sql
SELECT columns
FROM table as t1
JOIN table as t2
ON t1.colName = t2.colName;
```
ex : 
![[Pasted image 20251001233436.png]]
### Cross Join 
![[Pasted image 20251001221322.png]]

It combines each row of the first table with every row of the second table.

It will perform a cartesian product of A table with B table
All possible combinations will be the output
Final table will have `m*n` rows

```sql
SELECT *
FROM table1
CROSS JOIN table2;

--ex : 
SELECT * 
FROM customer
CROSS JOIN orders;
```

## Exclusive Self Join
Exclusive joins are used when we want to retrieve data from two tables excluding matched rows. 
They are a part of outer joins or full outer join.

Types of Exclusive Joins : 
![[Pasted image 20251002103651.png]]

### Left Exclusive Join : 
When we retrieve records from the left table excluding the ones matching in both left and right table.

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.colName = table2.colName
WHERE table2.colName IS NULL;

-- EX :
SELECT *
FROM customer
LEFT JOIN orders
ON customer.id = orders.id
WHERE orders.id IS NULL;
```

### Right Exclusive Join
When we retrieve records from the right table excluding the ones matching in both left and right table.

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.colName = table2.colName
WHERE table1.colName IS NULL;

--EX : 
SELECT *
FROM customer
LEFT JOIN orders
ON customer.id = orders.id
WHERE customer.id IS NULL;
```


### Full Exclusive Join
When we retrieve records from the right table and left table excluding the ones matching in both left and right table.

Perform LEFT EXCLUSIVE  JOIN and RIGHT EXCLUSIVE JOIN and perform union on them

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.colName = table2.colName
WHERE table2.colName IS NULL

UNION 

SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.colName = table2.colName
WHERE table1.colName IS NULL;

--EX : 
SELECT *
FROM customer
LEFT JOIN orders
ON customer.id = orders.id
WHERE orders.id IS NULL

UNION

SELECT *
FROM customer
LEFT JOIN orders
ON customer.id = orders.id
WHERE customer.id IS NULL;
```


## UNION operator in SQL
Union operator in SQL is used to combine the results of two or more SELECT queries into a single result set and gives unique rows by removing duplicate rows.

Things to keep in mind : 
1. Each SELECT command within the UNION must retrieve the same number of columns.
2. The data types of columns in corresponding positions across SELECT statements should match
3. Columns should be listed in the same order across all SELECT statements.

### UNION ALL Operator in SQL
UNION Operator in SQL is used to combine the results of two or more SELECT queries inot a single result set and gives all rows by **not removing duplicate rows**.

```sql
SELECT columns
FROM table1
UNION ALL
SELECT columns
FROM table2;
```

# SUBQUERIES IN SQL
SQL subquery is a query nested within another SQL statement. 
Whenever we want to retrieve data based on the result of another query we use nested queries.

Using SUBQUERIES : 
1. Subqueries can be used with clauses such as SELECT, INSERT, UPDATE or DELETE to perform complex data retrieval
QUERY : 
```SQL
SELECT columns, (subquery)
FROM tableName;

--EX : 
--Find the employees with average age and age of employees
SELECT AGE, (AVG(age) FROM employee) as avg_age
FROM employee;
```

2. Subqueries can be used with WHERE clause to filter data based on conditions.
```sql
SELECT *
FROM tableName
WHERE column name operator (subquery);

--ex : 
-- Find all the employees who have salary greater than the min salary
-- 1. find the min salary
-- 2. find employee having salary greater than min salary
SELECT name, salary
FROM employee
WHERE salary >(SELECT MIN(salary) FROM employee);

-- Find the employee with the minimum age
-- 1. FInd the min age
-- 2. Find employee having the min age
SELECT name, age 
FROM employee
WHERE age = (SELECT MIN(age) FROM employee);
```

 3. Subqueries can also be used in the FROM clause
```sql
SELECT *
FROM subquery AS altName;

-- ex : 
-- To find the employee with age greater than  the minimum age
SELECT emp.name, emp.age
FROM employee emp, (SELECT MIN(age) AS min_age FROM employee) AS subquery
WHERE emp.age > subquery.min_age;
```

# QUESTIONS
1. Find the nth highest salary in a given dataset
Steps 
- Select the column which you want to show the final result i.e. salary
- Order the salary in descending order so that you have the max at the first
- Now the value of n could be 1,2,3, ... n. So we have to make the query in such a way that whatever be the value of n, it can provide the result.
- So at the end of the query we will provide a LIMIT so that on the dataset which we have got after ordering the salary in descending order, we can fetch the nth highest one.

```sql
SELECT DISTINCE Salary
FROM tableName
ORDER BY Salary DESC
LIMIT n-1, 1; -- skip first n-1 and get the 1 after that
```


## Stored Procedures
These are programs that can perform specific tasks based on the stored query
It is basically a collection of pre-written SQL statements grouped together under a specific name

```SQL
-- Query to create a procedure
CREATE PROCEDURE procedureName()
BEGIN
Query
END;

-- Query to call the procedure
CALL procedureName(); 
```

```sql
-- Example for a stored procedure without params
DELIMITER /
CREATE PROCEDURE getAllOrderDetails()
BEGIN 
Select * from orders;
END /
DELIMITER ;

-- query to call the procedure 
CALL getAllOrderDetails();
```

DELIMITER -> sets the delimiter as / temporarily and reset it after the end


```sql
-- Example to return the detail sof order by id (Stored procedure with params)
CREATE PROCEDURE getAllOrderDetailsById(IN id int)
BEGIN
SELECT * FROM Orders WHERE id = id;
END;

-- calling the procedure
CALL getAllORderDetailsById(2);
```


# Views in SQL
A view is a virtual table in SQL
It helps in providing a filtered view of data for security purposes
```sql
CREATE VIEW viewName AS
SELECT columns FROM baseTableName (Specify the columns to be included in the view)

-- ex : 
CREATE VIEW employeeView AS
SELECT id, name, city FROM employee;
```
Views help in Data Abstraction, Security and to simplify complex queries.

ex : not display user_password in a new table which would be visible to other users 

Since view is a virtual table, all the operations we perform on tables can be performed on views

```sql
SELECT * FROM viewName;
DROP VIEW IF EXISTS viewName;
etc.

-- ex : 
SELECT id FROM employeeView;
```

--- 