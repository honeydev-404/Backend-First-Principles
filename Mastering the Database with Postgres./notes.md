9. Mastering The Database With Postgres.

What is Database and & Data Persistence :-
* In the simplest sence,a database is a structured way of storing and managing data. Your phone’s contact list is a database and all the cookies or key-value pairs are also databases, even your plain text file also comes in this as a very basic database.
* Data persistence means data remains available even after a shutdown or a reboot of the application or server. If the data survives after the reboot then we can say that there is use of persistent storage in the server.
* Backend applications need persistant storage for various reasons such as :-
    * To save the session data of the users.
    * To prevent data loss.
    * To keep the things running after the reboot.
* If the data survives after the application/session terminates then the use of persistent storage is being used in it.

Storage: RAM vs Disk :-
* There are two types of memory ->
    1. Primary Memory (RAM): It is extremely fast depends on the generation of the Ram, it is also volatile means if a server reboots then the data will be erased means it is non-persistent storage. And it is also less in quantity.
    2. Secondry Memory (HDD and SSD): In secondry memory there are of two types first is Hard Disk Drive (HDD) which is comparetively slower than SSD. Solid State Drive (SSD) it is the faster mode of storage and both of these are used for the file storage and they in large capacities but they are comparatively slower but they are persistant, so they will not lose data after a shutdown or reboot.
* The traditional databases use persistent disk storage because they are large in quantity and but they are persistent.
* The Ram is used for caching because of its speed and it is mostly used by the services called Redis because it is used to store session data so that we can save the time for getting information from the secondry storage so we can store the most requested information in the cache/ram so we can give the information as soon as possible.

Why Not Simply Text/Flat Files :-
* There are various reasons for why we are not simply using text files :-
    1. The data or the files will be difficult to find and so does modifying data.
    2. They will lack proper structure or schemas. And it is very Error-Prone.
    3. There will be data inaccuracy, validation and data integrity will be difficult to maintain.
* There will be no concurrency control means if two users update the same field at once the one who saved at last will win but that one update will be silently lost / no consistency.
* The Database Management System (DBMS) Solves this problem by giving us efficient ways to perform CRUD operations.

Database Management System (DBMS) :-
* DBMS is a software that manages databases and allows us to perform many  CRUD operations and it allows the application to interact with database. It also have some core responsibilities ->

Responsibilites 	What it means
Data organization 	Lay data out so fetch/update/insert are efficient.
Access	Provides methods for CRUD operations.
Integrity	Keep data accurate and valid. Ex -> a payment field accepts only numbers, never a string.
Security	Protect data from unauthorized access via users or roles.

Relational Database :-
* A Relational Database(RDBMS) organizes the data into a table consist of columns and rows and the tables are connected by the foreign keys. They are strict and has a predefined schemas as we are making a table we have to tell the data type of the field nothing happens “on the fly”. This strictness buys a big payoff, Data Integrity: at any moment you can bet on its shape and types of the data.
* Structures Query Language(SQL) is a language which helps us retrive,delete,create or modify data on the relational database. For example :- Postgres, SQLite, MySQL.

Non-Relational Database :-
* A NoSQL databases like have no predefined schemas or structure and they are stored in many formats such as JSON (It only looks like that). Its advantage is flexibility: great when prototyping fast and when youu genuinely don’t know the data’s shape or data type in advance. And the cost is weaker integrity: with no schemas enforced at the database level it adds more complexity and it is more error-prone.
* MongoDB is an software which is used to manage or organize non-relational databases

Picking With Two Examples :-
* CRM (Customers Relationship Management) -> relational. Contact, sales, opportunities, and there relationship are critical and needs a proper structure, demands strong integrity. Postgres is a great fit. 
* CMS (Content Management System) -> non-relational. An articles may contain multimedia, block of code and the variety of content you can’t pin to a fixed schemas. “Take it all and dump it” suits in this.

Why Choose PostgreSQL :-
* PostgreSQL is a open-source software that is used to manage relational databases and and it is known for its integrity and strictness in terms of data types. It also meets most of the modern backend requirement and supports different data types.

PostgreSQL Data Types :-
* Numeric Types ->
TYPE	When to use
smallint	Status codes, human age.
integer	Standard counts, primary keys.
bigint	Large number traffic ID’s , Big data.
decimal/numeric	Always use for money/price (exact).
Floating-point types	Approximate decimal values.

* String Types ->
TYPE	When to use
char	Country codes, it has fixed length.
varchar	Name or email address.
text	Variable-Length, no limit. prefer this is postgres.

* Date & Time Types :-
TYPE	When to use
date	birthday, hire dates.
time	exact time (09:00:00).
timestamp	time without time zone.
timestamptz	order creation timestamps.
interval	countdown timers.

* Other Important Types :-
TYPE	When to use
BOOLEAN	toggles (is_active).
UUID	primary keys in distrubuted systems.
JSON	json data
JSONB	Decomposed json representation
ARRAYS	phone numbers and role lists.
Custom ENUM Types	useful for state (“pending” , “complete”, etc).

Basic SQL Knowledge to Proceed Further :-
* Column: A named field, such as name or age.
* Row: One record in a table, such as Honey's record.
* Table: A collection of rows and columns.

* Basic Queries :-
    1. SELECT -> Fetch all columns and rows from the table.
    2. FROM -> It tells from which table to extract data.
    3. WHERE -> It helps to extract data via a conditon to extract data that is needed only rather than extracting all the rows and columns, like name="Bobby”.
    4. UPDATE -> It helps us to update data in the database.
    5. DELETE -> It is used to delete data from the database.
* Query Modifiers :-
    1. ORDER BY -> This keyword is used to sort result-set in ascending or descending or by other fields.
    2. ASC -> To sort the result in Ascending(small -> big) order use ASC.
    3. DESC -> To sort the result in Descending(big -> small) order use DESC.
    4. LIMIT -> Restricts the maximu number of records a query can return. Example -> LIMIT 5 ensures you get no more rows than 5 rows.
    5. OFFSET -> Specifies how many rows to skip at each sets. Example -> OFFSET 10 skips first 10 rows.
* Table Creation :-
    * CREATE TABLE -> It is used to create tables and assign table’s name, define its data types and set of rules (Constraints).
    * Columns and there data types -> Every table columns must be assigned a specific data type that defines what kind of value it holds. And the types are :-
        1. Numberic Data Types.
        2. String And Character Data Types.
        3. Date & Time Data Types.
        4. Binary & Advanced Data Types.
* Constraints :-
    1. PRIMARY KEY -> It is used to uniquely identify a specific record in a table. Like if there is a customer and his name is Bobby then we will assign him a customerID so that customerID will be Primary Key. Null is strictly prohabitated. And it is completely uniques no duplicate values are allowed.
    2. FOREIGN KEY -> It establishes a link between two tables and prevents the action that will destroy the liink between them. A FOREIGN KEY is a column in a table that refers to a PRIMARY KEY in another table. Null values are allowed, and duplicate values can exist. Primary key from one table can point to many tables.
    3. NULL -> In DB, Null means unknown,missing or no data to provide to the field it doesn’t simply means that it is 0(zero). It literally means nothing is provided.
    4. NOT NULL -> In most of the DB, the NULL values are allowed but when you add NOT NULL constraints, then when you will add a row or update it then that column must have a value. If you try to save a row without a value for that column, then the database will reject it and throw an error.
    5. UNIQUE -> This constraint is used to ensure that all the values in the column or a group of column are completely different from one another. No two rows can have exact same values. It allows null because it describes that there is an unknown value not simply 0 everywhere. A table can have a one PRIMARY KEY but it can have as many UNIQUE constraints as you need.
    6. CHECK -> This constraint allows to validate the input of the users like a boolean expression: TRUE or FALSE. If condition evaluates to FALSE, then the database will reject the insert or update.

Database Modelling :-
* Database modelling is the process of translating real world bussiness logic into a structured,performant and reliable storage schema.
* Converting entities into tables is the systematic tranformation of bussiness nouns,attibutes and relationships into SQL schemes with strict data types, primary keys and foreign keys.
* Choosing columns and data types is also one of the major part of database modelling so we have to make it performant and easy to understand rather than just dumping the data.

Table Relationships & Referential Integrity :-
* Table Relationship is a logical link between two or more tables. It defines how data in one table is connected to others, preventing duplicates records and reducing complexity and keeping data organized.
* There are Three Core Types of Relationships :-
    1. One-On-One (1:1) -> It means that a single record in Table A is linked exactly one record in Table B and vice versa. They have this relationship by one primary key in one table and a foreign key in another table, Example -> tables user and user_profiles, where each user has a single corresponding profile.
    2. One-TO-Many (1:N) -> Each record of the table A is accociated with multiple records in table B, but each record in table B is associated with only one record in table A. Include a “many” Foreign key in table B that references the promary key of the “one” side table A. Example -> A department can have multiple employees, but each employee belongs ot one departement.
    3. Many-To-Many (M:N) -> Each record in Table A can be associated with Table B and vice versa, create a intermediate table (also known as a junction or linking table) that contains foreign keys referencing both related data. Example -> Tables student and cources, where each student enroll in multiple cources , and each cources can have multiple students.
* Refrential Integrity -> It is a database rule that guarantees relationships between tables always remain valid and consistent. When the data gets disconnected from the user and breaking backend code and causes crashing then it is called orphaned data. This problem is solved by the Foreign key it says “Before you let anyone update or insert a row in orders, check the users table first. If the user_id does not exist in user.id, reject the query immediatly.”

Database Migrations :-
* Database migration refers to managing and applying changes to a database schema or moving data between databases over time.
* Just like git tracks changes to code over time, migration tracks structural changes like tables, columns or constraints to your database.
* Plain text/SQL scripts files containing specific instructions for a single change. Example -> 20261001_create_users.sql .
* Migration is useful in backend developement because of team sync, automated deployments and reproducibility.

Migration Tools :-
* dbmate -> A lightweight, language-agnostic cli tool for managing database  schemas using raw SQL.
* A single generated .sql file split into two distinct blocks. —migrate:up , —migrate:down.

Up Migration :-
* Role -> Apply new changes forward.
* Action -> Creates tables, adds/modifies columns or builds new indexes.

Down Migration :-
* Role -> Revert changes backwards.
* Action -> Exactly undoes what the UP step did to roll back to safety if a release fails.

Migration Workflow :-
1. Migration (Write file).
2. UP (Apply).
3. Database Changes.
4. DOWN (Revert Changes).
5. Rollback (Undoes exactly what the UP did).

Database Seeding :-
* Database seeding means running an automated scripts that inject initial “starter data” into the database with a single command.
* It is used if we are building a database from scratch and want some initial/dummy values to test its workings.
* Seed data is the initial,baseline data provided to a database when it first set up or reset.
* Instead of typing random, unpredicatabe values every day, seed data provides a fixed deterministic baseline.
* Data must go in parent-first, child-second.
* Using SQL to populate the database by executing a seed script which contains batches of insert statements designed to fill tables in seconds.
* CTEs completely prevents guessing IDs, stops foreign key errors, and links parent-child rows together in a single atomic command.

API Querying & Joins :-
* Database queries from backend API is very important concept as it tells the SQL to execute which commands to retrive or fullfill user’s demand.
* The flow looks like :- API Request -> Backend -> SQL -> Database -> Result.
* Joins lets us combine related data from both table in one queries. Suppose there is a Owner_ID in table A and ID in table B so it is a foreign key relationship. It is used to avoid writing duplicate values again and again and provides data consistency.
* Inner Join -> It is same as join , so the definition of inner join -> it is used to join all the matching values from the left table to the right table and if there is no value assigned then it will be considered as null values and inner join allows null values. Returns only the matching row from both of the tables. Unmatched rows are excluded, not returned with null.
* Left Join -> It is used to return all the values from the left table and match them with right table and if value is not defined then it will be null or empty space.

Parameterized Queries :-
* Queries written by using a variable marker rather than just concatenating raw user input directly into the query string.
* Positional markers such as { $1 or $2 in Postgres and ? in MySQL }  represent where the values should be actually injected.
* Backend example -> await db.query(“SELECT * FROM users WHERE email = $1", [usersEmail] );
* SQL injection is a type of attack in which an attacker tries to escape the boundries of double quotes and and execute that as a command not as a data entry.
* It can be prevented by using position markers because they will treat the user input as the litreal values not like executable commands.

Dynamic Filtering :-
* WHERE -> Used to apply a condition so that we can retrive only the required data. For Example (WHERE name = "Bobby”)
* AND -> All chained conditions must be true to return the row.
* OR -> At least one condition must be true then it will return the row.
* Safe Sorting -> It is a safety mechanism that prevents SQL injection in the ORDER BY and more clause and doesn’t allow custom instructions to reach to these clause. Think of it like a restrurant menu: user can only pick what is mentioned in the menu so in this case the user can only erxecute the commands which are mentioned by the application instructions. It is safe as it restricts custom instruction like DROP TABLE users.
* LIMIT -> Restricts the maximum rows that will be returned.
* OFFSET -> Skips the set of rows before returnin the results. OFFSET = (page -1) * limit .

Triggers :-
* Definition -> A named database mechanism that automatically executes a function when a certian data modification occurs in table.
* Trigger vs. Function -> The trigger defines a event listener that decided when to execute the function and on which table. The trigger function contains procedural code that gets in action when trigger activates.
* BEFORE VS. AFTER :- 
    * BEFORE -> Runs prior before writing to disk, used to validate and alter(to change , modify or interfere with something) data before saving.
    * AFTER -> Runs after changes are written, used for audit logging.

Indexing :-
* It is a database optimization technique that creates a seperate, highly organised data structure resulting in faster data retrival operations.
* A B-Tree (Balanced Tree) is a self-balancing data structure designed to handle large amount of data efficiently.
* Benefits -> It dramatically accelerates read speed with WHERE, ORDER BY and JOIN clauses in operations.
* Trade-Off -> It consumes extra space and slows down write operations because indexes must be updated whenever the data of the table changes.
 


