# DATABASE BASICS

## Title: Table
### Description:
Where all the data in a database is stored. A database consists of one or more tables.  Each table is made up of rows and columns. Columns are defined to hold specific type of data. Rows contain the set of data.
### Implementation:
```sql
CREATE TABLE Persons (
	PersonID int,
	LastName varchar(255),
	FirstName varchar(255),
	Address varchar(255),
	City varchar(255) 
);
```
### Code Explanation:
Creates a table called Persons which has columns PersonId, LastName, FirstName, Address and City. Also it defines the type of and size of the data to be input.
 
=================

## Title: Trigger
### Description:
It is a special stored procedure that gets run when specific actions occur within a database. Usually they are defined to run when changes are made to a table’s data.
### Implementation:
```sql
CREATE [OR REPLACE ] TRIGGER trigger_name 
{BEFORE | AFTER | INSTEAD OF } 
{INSERT [OR] | UPDATE [OR] | DELETE} 
[OF col_name] 
ON table_name 
[REFERENCING OLD AS o NEW AS n] 
[FOR EACH ROW] 
WHEN (condition)  
DECLARE
Declaration-statements
BEGIN 
Executable-statements
EXCEPTION
Exception-handling-statements
END;
``` 
### Code Explanation:
CREATE [OR REPLACE] TRIGGER trigger_name − Creates or replaces an existing trigger with the trigger_name.
{BEFORE | AFTER | INSTEAD OF} − This specifies when the trigger will be executed. The INSTEAD OF clause is used for creating trigger on a view.

{INSERT [OR] UPDATE [OR] DELETE} − This specifies the DML operation.

[OF col_name] − This specifies the column name that will be updated.

[ON table_name] − This specifies the name of the table associated with the trigger.

[REFERENCING OLD AS o NEW AS n] − This allows you to refer new and old values for various DML statements, such as INSERT, UPDATE, and DELETE.

[FOR EACH ROW] − This specifies a row-level trigger, i.e., the trigger will be executed for each row being affected. Otherwise the trigger will execute just once when the SQL statement is executed, which is called a table level trigger.

WHEN (condition) − This provides a condition for rows for which the trigger would fire. This clause is valid only for row-level triggers.

==================

## Title: Index
### Description:
A database index allows a query to efficiently retrieve data from a database.  Indexes are related to specific tables and consist of one or more keys.  A table can have more than one index built from it.  The keys are the values we want to look up in the index.  The keys are based on the tables’ columns.  By comparing keys to the index it is possible to find one or more database records with the same value.
### Implementation:
```sql
CREATE INDEX idx_lastname ON Persons (LastName);
```
### Code Explanation:
It makes an index called idx_lastname on the Persons table using LastName as its key.
 
================= 

## Title: View
### Description:
A database view is a searchable object in a database that is defined by a query. It is sometimes called a virtual table, but it does not store any data. A view can combine data from multiple tables by using joins or it can simply be subset of one table.
### Implementation:
```sql
CREATE VIEW NewBooks AS
SELECT ISBN, Title, Author
FROM Books
WHERE IsNew = 1
```
### Code Explanation:
We create views by using the CREATE VIEW command followed by a query that is used to construct the virtual table we want.
 
================

## Title: Cursor
### Description:
SQL is a set-based language. This means operations are completed on all rows of a result. But when one wants to do an operation row by row, database cursors are used.
A cursor is like a pointer to a specific row within a query result. It can be moved from one row to the next (or the previous row depending on the type of cursor). There are two main categories of cursors: scrolling capabilities and ability to detect changes made to the database.
The reason you may need to use a database cursor is that you need to perform actions on individual rows.

For example, consider this update statement:
UPDATE EmployeeTable
SET	City = 'Dire Dawa'
WHERE  EmployeeID < 10031
It updates every row in the table EmployeeTable where the EmployeeID is less than 10031.  If, during the update operation, there is an error, then no rows are updated.  The entire update is treated as a transaction.
But using a cursor, we iterate over the rows and updating them as we go. Rows will be updated independently.
 
### Implementation:
```sql
DECLARE @businessEntityID as INT;
DECLARE @firstName as NVARCHAR(50),
    	@lastName as NVARCHAR(50);
DECLARE @personCursor as CURSOR;
 
SET @personCursor = CURSOR FOR
    SELECT BusinessEntityID,
       	FirstName,
       	LastName
    FROM  Person.Person
    OPEN @personCursor
FETCH NEXT FROM @personCursor INTO @businessEntityID,
                               	@firstName,
                               	@lastName
WHILE @@FETCH_STATUS = 0
BEGIN
   PRINT cast(@BusinessEntityID as VARCHAR (50))
     	+ ' - ' + @firstName
     	+ ' ' + @lastName;
   FETCH NEXT FROM @personCursor INTO @businessEntityID,
          	                        @firstName,
                                  	@lastName
END
CLOSE @personCursor;
DEALLOCATE @personCursor;
```
### Code Explanation:
1. Declare Variables
2. Declare Cursor
3. Fetch values into variables
4. Test Status and Loop
5. Close Cursor
6. Deallocate Cursor
 
=================
 
## Title: Record
### Description:
A database record refers to a complete set of information, a collection of fields about the same person/item/object.  A database record can be thought of as a row of information within a database table.
### Implementation:
```sql
SELECT * FROM EmployeeTable
```
### Code Explanation:
It selects and returns the complete set of records within the EmployeeTable. Each row within the result from the query is a record.
 
=================

## Title: Package
### Description:
A package is a group of related procedures and functions, together with the cursors and variables they use, stored together in the database for continued use as a unit. Similar to standalone procedures and functions, packaged procedures and functions can be called explicitly by applications or users.
### Implementation:
```sql
CREATE PACKAGE bank_transactions  AS

	/* Declaration of Procedures, 
	Cursors, 
	Functions, 
	Variables…  */
	
	minimum_balance  CONSTANT NUMBER := 100.00;
	PROCEDURE enter_transaction (acct   NUMBER, kind   CHAR, amount NUMBER);
  
END bank_transactions;
 
CREATE PACKAGE BODY bank_transactions AS

	/* This is where the definitions of the declarations we made above is put */
 
END bank_transactions;
```

### Code Explanation:
There are two parts to a package. First part is declaration. Declaration of related procedures, variables, and cursors. Second part is definition. Definition of the above related procedures, variables, and cursors.
 
===================

## Title: Procedure
### Description:
Procedures are used as scripts. They run series of commands for you and you can schedule them to run at certain times. They don’t take arguments and don’t return values.
### Implementation:
```sql
CREATE PROCEDURE HumanResources.uspFindEmployee
	@BusinessEntityID [int]
AS
BEGIN
	SET NOCOUNT ON;
	SELECT BusinessEntityID,
		NationalIDNumer,
		LoginID,
		JobTitle,
		HireDate
	FROM HumanResources.Employee
	WHERE BusinessEntityID = @BusinessEntityID
END
           	
```
### Code Explanation:
First the procedure name is stored. Then the variable parameter is declared. The query that is to be run is stated using the parameter we declared earlier inside of it (the query). Then we end the procedure with END

==================

## Title: Function
### Description:
Functions are used as methods. You pass it something and it returns a result. Should be small and fast - does it on the fly. Functions can’t call stored procedures, but stored procedures can call functions.
### Implementation:
```sql
CREATE FUNCTION MyFunction (@someValue INTEGER) RETURNS INTEGER
AS
BEGIN
   DECLARE @retval INTEGER
 
   SELECT localValue
  	FROM dbo.localToNationalMapTable
      WHERE nationalValue = @someValue
 
   RETURN @retval
END
 ```
### Code Explanation:
First the function name is stored. We pass it a parameter and specific the return type. The return variable is declared. The query is run using the return variable we declared. Then we return the variable and end the procedure with END.

====================
 
## Title: Object
### Description:
A database object in a relational database is a data structure used to either store or reference data. The most common object that people interact with is the table. Other objects are indexes, stored procedures, sequences, views and many more. An object type is the concept of the object. Like the concept of a table or index. The object instance is an instance of the object. A certain table, for example.
 
==================== 
 
## Title: Database Link
### Description:
A database link is a schema object in one database that enables you to access objects on another database. The other database need not be the same Database system. After you have created a database link, you can use it to refer to tables and views on the other database. In SQL statements, you can refer to a table or view on the other database by appending @dblink to the table or view name. You can query a table or view on the other database with the SELECT statement. You can also access remote tables and views using any INSERT, UPDATE, DELETE, or LOCK TABLE statement.
### Implementation:
```sql
CREATE DATABASE LINK local
CONNECT TO hr IDENTIFIED BY hr
USING 'local';
 ```
### Code Explanation:
Create database link using CREATE DATABASE LINK and the specifying the database we are connecting to.

==================

## Title: Sequence
### Description:
This database object is used to create a sequence in database. A sequence is a user created database object that can be shared by multiple users to generate unique integers. A typical usage for sequences is to create a primary key value, which must be unique for each row. The sequence is generated and incremented (or decremented) by an internal Oracle routine.
### Implementation:
```sql
CREATE SEQUENCE dept_deptid_seq
                    	INCREMENT BY 10
                    	START WITH 120
                    	MAXVALUE 9999
                    	NOCACHE
                    	NOCYCLE;
```
### Code Explanation:
Create sequence, give it a name and then define the increment value, the starting value, the max value. We specify CYCLE to indicate that the sequence continues to generate values after reaching either its maximum or minimum value. After an ascending sequence reaches its maximum value, it generates its minimum value. After a descending sequence reaches its minimum, it generates its maximum value. And cache specifies how many values of the sequence the database preallocates and keeps in memory for faster access. 



====================

## Title: Constraint
### Description:
Constraints are the rules enforced on the data columns of a table. These are used to limit the type of data that can go into a table. This ensures the accuracy and reliability of the data in the database.
- Primary Key - The primary key consists of one or more columns whose data contained within is used to uniquely identify each row in the table.  You can think of the primary key as an address.  If the rows in a table were mailboxes, then the primary key would be the listing of street addresses. When a primary key is composed of multiple columns, the data from each column is used to determine whether a row is unique.
- Foreign Key - A foreign key is a set of one or more columns in a table that refers to the primary key in another table.  There isn’t any special code, configurations, or table definitions you need to place to officially “designate” a foreign key.
- Not Null - Ensures that a column cannot have NULL value.
- Check - The CHECK constraint ensures that all the values in a column satisfies certain conditions.
- Unique key - Ensures that all values in a column are different.
### Implementation:
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int CHECK (Age>=18)
);
 
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);
 
CREATE TABLE Persons (
    ID int NOT NULL UNIQUE,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int
);
```
### Code Explanation:
We use these constraints when we are creating Tables and these constraints are the rules governing what kind of data is acceptable in those columns. The code above shows normal table creation with the use of constraints.
- NOT NULL Constraint − Ensures that a column cannot have NULL value.
- UNIQUE Constraint − Ensures that all values in a column are different.
- PRIMARY Key − Uniquely identifies each row/record in a database table.
- FOREIGN Key − Uniquely identifies a row/record in any of the given database table.
- CHECK Constraint − The CHECK constraint ensures that all the values in a column satisfies certain conditions.

 

