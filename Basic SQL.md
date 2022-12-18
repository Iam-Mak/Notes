# Basic SQL
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

### Types of Statements
- **CREATE TABLE** is a statement that creates a new table in a database.
- **DROP TABLE** is a statement that removes a table in a database.
- **SELECT** allows you to read data and display it. This is called a query.

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
