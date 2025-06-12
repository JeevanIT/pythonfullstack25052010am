To create a new user in SQL Server and connect using that user:
________________________________________
🔸 1. Create a Login at the Server Level
CREATE LOGIN Jeevan WITH PASSWORD = 'Jeevan123!';
•	Jeevan: The username you want to create.
•	'Jeevan123!': Use a strong password.
________________________________________
🔸 2. Create a User in a Specific Database
Switch to the database where you want to grant access:
USE MyDatabase;
CREATE USER MyUserjeevan FOR LOGIN Jeevan;
________________________________________
🔸 3. Assign Roles or Permissions
Give the user read/write access by assigning a role:
-- For read-only access:
EXEC sp_addrolemember 'db_datareader', ' MyUserjeevan ';

-- For read and write access:
EXEC sp_addrolemember 'db_datawriter', ' MyUserjeevan ';

-- For full DB access:
EXEC sp_addrolemember 'db_owner', ' MyUserjeevan ';
________________________________________
🔸 4. Connect to SQL Server with New User
•	Open SQL Server Management Studio (SSMS).
•	Click Connect → Database Engine.
•	Set:
o	Server Name: your SQL Server instance (e.g., localhost\SQLEXPRESS or IP address).
o	Authentication: SQL Server Authentication.
o	Login: Jeevan
o	Password: Jeevan123!


Show logins that exist at the server level
-- All SQL and Windows logins
SELECT  name,                                    -- login name
        type_desc,                               -- SQL_LOGIN | WINDOWS_LOGIN | WINDOWS_GROUP
        is_disabled,                             -- 1 = disabled
        default_database_name,
        create_date,
        modify_date
FROM    sys.server_principals
WHERE   type IN ('S', 'U', 'G')                  -- S = SQL login, U = Windows login, G = Windows group
ORDER BY name;

Only SQL-authenticated logins:

SELECT name
FROM   sys.server_principals
WHERE  type = 'S';   -- SQL_LOGIN

Classic system procedure:
EXEC sp_helpuser;        -- returns users, roles, and default schemas

Check Users: 
Who am I right now?
SELECT ORIGINAL_LOGIN(); 
SELECT  SYSTEM_USER; 
SELECT  CURRENT_USER;

Alter Logins and Users: 
Change Login Password:
ALTER LOGIN [YourLoginName] WITH PASSWORD = 'NewPassword123!';
Enable and Disable Login: 
-- Disable a login
ALTER LOGIN [YourLoginName] DISABLE;
-- Enable a login
ALTER LOGIN [YourLoginName] ENABLE;



Alter Users:
ALTER USER [YourUserName] WITH LOGIN = [YourLoginName];
Change User Name:
ALTER USER [OldUserName] WITH NAME = [NewUserName];

Task	Command Example
Change login password	ALTER LOGIN [user] WITH PASSWORD = 'new';
Enable/disable login	ALTER LOGIN [user] ENABLE;
Change default DB	ALTER LOGIN [user] WITH DEFAULT_DATABASE = [DBName];
Map user to login	ALTER USER [user] WITH LOGIN = [login];
Change user schema	ALTER USER [user] WITH DEFAULT_SCHEMA = [schema];

Delete Logins and Users
Action	SQL Command
Drop a user	DROP USER [UserName];
Drop a login	DROP LOGIN [LoginName];


Database Commands:

Create database:
CREATE DATABASE [Database Name];

Use the new Database: 
USE [Database Name];

Check Existing Databases: 
SELECT name, database_id, create_date 
FROM sys.databases;

Delete (Drop) a Database:
DROP DATABASE [DatabaseName];

Alter (Modify) a Database: -
ALTER DATABASE SchoolDB MODIFY NAME = NewSchoolDB;


Table Commands
Action	Command Example
Create Table	CREATE TABLE Students (...)
Add Column	ALTER TABLE Students ADD Email VARCHAR(100);
Modify Column	ALTER TABLE Students ALTER COLUMN Marks FLOAT;
Drop Column	ALTER TABLE Students DROP COLUMN Email;
Drop Table	DROP TABLE Students;
Rename Table	EXEC sp_rename 'OldName', 'NewName';
Rename Column	EXEC sp_rename 'Table.Column', 'New', 'COLUMN';


Create a Table: 
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    BirthDate DATE,
    Marks INT
);

Alter a Table:
You can add, modify, or drop columns and constraints.

ALTER TABLE Students ADD Email VARCHAR(100);

Modify a Column (e.g., change data type)
ALTER TABLE Students ALTER COLUMN Marks FLOAT;

Drop a Column
ALTER TABLE Students DROP COLUMN Email;

Delete (Drop) a Table
DROP TABLE Students;

Rename a Table 
EXEC sp_rename 'Students', 'StudentInfo';

Rename Column
EXEC sp_rename 'StudentInfo.Marks', 'TotalMarks', 'COLUMN';


Insert Commands
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Marks INT
);

INSERT INTO Students (StudentID, FirstName, LastName, Marks)
VALUES (1, 'John', 'Doe', 85);

INSERT INTO Students (StudentID, FirstName, LastName, Marks)
VALUES 
(2, 'Jane', 'Smith', 90),
(3, 'Alex', 'Brown', 78),
(4, 'Sara', 'Lee', 88);


SQL Server command types: DML, DDL, DCL, TCL (often miswritten as DLL/DTCL)

Type	Full Form	Example Commands	Purpose
DDL	Data Definition Language	CREATE, ALTER, DROP, TRUNCATE	Define DB schema
DML	Data Manipulation Language	SELECT, INSERT, UPDATE, DELET
E	Manage data
DCL	Data Control Language	GRANT, REVOKE	Control user permissions
TCL	Transaction Control Language	COMMIT, ROLLBACK, SAVEPOINT, BEGIN TRAN	Manage transactions

1. DDL – Data Definition Language
Purpose: Defines the structure/schema of the database.
Command	Description
CREATE	Creates new database objects like tables, views, etc.
ALTER	Modifies an existing object (e.g., add a column to a table).
DROP	Deletes database objects permanently.
TRUNCATE	Removes all records from a table quickly without logging.
RENAME	Renames a database object. (Supported in some RDBMSs)
🔸 Example:
CREATE TABLE Employees (
  ID INT,
  Name VARCHAR(50),
  Salary DECIMAL(10,2)
);
________________________________________


 2. DML – Data Manipulation Language
Purpose: Used to manage data within schema objects.
Command	Description
SELECT	Retrieves data from one or more tables.
INSERT	Adds new records into a table.
UPDATE	Modifies existing records.
DELETE	Removes existing records from a table.
🔸 Example:
INSERT INTO Employees (ID, Name, Salary)
VALUES (1, 'John Doe', 50000.00);
________________________________________
3. DCL – Data Control Language
Purpose: Controls access and permissions to the database.
Command	Description
GRANT	Gives user access privileges.
REVOKE	Removes user access privileges.
🔸 Example:
GRANT SELECT, INSERT ON Employees TO user1;
________________________________________
4. TCL – Transaction Control Language
Purpose: Manages changes made by DML commands and ensures data integrity.
Command	Description
COMMIT	Saves all changes made during the transaction.
ROLLBACK	Undoes changes made during the transaction.
SAVEPOINT	Sets a point to which you can roll back later.
BEGIN TRAN	Starts a transaction (specific to SQL Server).
🔸 Example:
BEGIN TRAN;
UPDATE Employees SET Salary = Salary + 5000 WHERE ID = 1;
COMMIT;
