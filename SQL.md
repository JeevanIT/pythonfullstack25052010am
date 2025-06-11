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
