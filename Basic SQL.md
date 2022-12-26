
- **SQL Server on Azure Virtual Machines (VMs)** - A virtual machine running in Azure with an installation of SQL Server. The use of a VM makes this option an **infrastructure-as-a-service (IaaS)** solution that virtualizes hardware infrastructure for compute, storage, and networking in Azure; making it a great option for "lift and shift" migration of existing on-premises SQL Server installations to the cloud.
- **Azure SQL Managed Instance** - A **platform-as-a-service (PaaS)** option that provides near-100% compatibility with on-premises SQL Server instances while abstracting the underlying hardware and operating system. The service includes automated software update management, backups, and other maintenance tasks, reducing the administrative burden of supporting a database server instance.
- **Azure SQL Database** - A fully managed, highly scalable PaaS database service that is designed for the cloud. This service includes the core database-level capabilities of on-premises SQL Server, and is a good option when you need to create a new application in the cloud.
- **Azure SQL Edge** - A SQL engine that is optimized for *Internet-of-things (IoT)* scenarios that need to work with streaming *time-series* data.

==============================================================================================
Some popular dialects of SQL include:
- Transact-SQL (T-SQL). This version of SQL is used by Microsoft SQL Server and Azure SQL services.
- pgSQL. This is the dialect, with extensions implemented in PostgreSQL.
- PL/SQL. This is the dialect used by Oracle. PL/SQL stands for Procedural Language/SQL.

## SQL statement types
SQL statements are grouped into three main logical groups:

- Data Definition Language (DDL)
     - CREATE Create a new object in the database, such as a table or a view.
     - ALTER Modify the structure of an object. For instance, altering a table to add a new column.
     - DROP Remove an object from the database.
     - RENAME Rename an existing object.

```
CREATE TABLE Product
(
    ID INT PRIMARY KEY,
    Name VARCHAR(20) NOT NULL,
    Price DECIMAL NULL
);
```
- Data Control Language (DCL)
     - GRANT	Grant permission to perform specific actions
     - DENY	Deny permission to perform specific actions
     - REVOKE	Remove a previously granted permission
 ```
 GRANT SELECT, INSERT, UPDATE
ON Product
TO user1;
```
- Data Manipulation Language (DML)
     - SELECT	Read rows from a table
     - INSERT	Insert new rows into a table
     - UPDATE	Modify data in existing rows
     - DELETE	Delete existing rows
 ```
 SELECT FirstName, LastName, Address, City
FROM Customer
WHERE City = 'Seattle'
ORDER BY LastName;

UPDATE Customer
SET Address = '123 High St.'
WHERE ID = 1;

DELETE FROM Product
WHERE ID = 162;

INSERT INTO Product(ID, Name, Price)
VALUES (99, 'Drill', 4.99);
```
### What is a view?
A view is a virtual table based on the results of a SELECT query. 
```
CREATE VIEW Deliveries
AS
SELECT o.OrderNo, o.OrderDate,
       c.FirstName, c.LastName, c.Address, c.City
FROM Order AS o JOIN Customer AS c
ON o.Customer = c.ID;
```
### What is a stored procedure?
A stored procedure defines SQL statements that can be run on command.
```
CREATE PROCEDURE RenameProduct
	@ProductID INT,
	@NewName VARCHAR(20)
AS
UPDATE Product
SET Name = @NewName
WHERE ID = @ProductID;
```


============================================================================================
#### Why Businesses Like Databases
- **Data integrity is ensured** - only the data you want to be entered is entered, and only certain users are able to enter data into the database.
- **Data can be accessed quickly** - SQL allows you to obtain results very quickly from the data stored in a database. Code can be optimized to quickly pull results.
- **Data is easily shared** - multiple individuals can access data stored in a database, and the data is the same for all users allowing for consistent results for anyone with access to your database.

## Parch & Posey Database
- In this, we will mostly be using the Parch & Posey database for our queries.
- Parch & Posey (not a real company) is a paper company and the database includes sales data for their paper.
- [Parch & Posey Database
](https://video.udacity-data.com/topher/2020/May/5eb5533b_parch-and-posey/parch-and-posey.sql)

## Entity Relationship Diagrams
![image](https://user-images.githubusercontent.com/92245436/208279091-e73e77ec-2708-4e47-bd29-d359f56d099a.png)

In the Parch & Posey database there are five tables (essentially 5 spreadsheets):

- web_events
- accounts
- orders
- sales_reps
- region

The "crow's foot" that connects the tables together shows us how the columns in one table relate to the columns in another table. 

### Introduction to Logical Operators
Logical Operators include:
- **LIKE** This allows you to perform operations similar to using *WHERE* and =, but for cases when you might not know exactly what you are looking for.
- **IN** This allows you to perform operations similar to using *WHERE* and =, but for more than one condition.
- **NOT** This is used with **IN** and **LIKE** to select all of the rows **NOT LIKE** or **NOT IN** a certain condition.
- **AND & BETWEEN** These allow you to combine operations where all combined conditions must be true.
- **OR** This allows you to combine operations where at least one of the combined conditions must be true.

#### Like
All the companies whose names start with 'C'.
```
SELECT name
FROM accounts
WHERE name LIKE 'C%';
```
All companies whose names contain the string 'one' somewhere in the name.
```
SELECT name
FROM accounts
WHERE name LIKE '%one%';
```
All companies whose names end with 's'.
```
SELECT name
FROM accounts
WHERE name LIKE '%s';
```

#### IN 
Use the accounts table to find the account name, primary_poc, and sales_rep_id for Walmart, Target, and Nordstrom.
```
SELECT name, primary_poc, sales_rep_id
FROM accounts
WHERE name IN ('Walmart', 'Target', 'Nordstrom');
```
#### AND and BETWEEN
Write a query that returns all the orders where the standard_qty is over 1000, the poster_qty is 0, and the gloss_qty is 0.
```
SELECT *
FROM orders
WHERE standard_qty > 1000 AND poster_qty = 0 AND gloss_qty = 0;
```
#### OR
Find all the company names that start with a 'C' or 'W', and the primary contact contains 'ana' or 'Ana', but it doesn't contain 'eana'.
```
SELECT *
FROM accounts
WHERE (name LIKE 'C%' OR name LIKE 'W%') 
           AND ((primary_poc LIKE '%ana%' OR primary_poc LIKE '%Ana%') 
           AND primary_poc NOT LIKE '%eana%');
```
