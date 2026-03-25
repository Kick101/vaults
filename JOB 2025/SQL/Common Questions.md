### **1. What is SQL?**

It stands for [Structured Query Language,](https://www.datacamp.com/blog/all-about-sql-the-essential-language-for-database-management) used for interaction with relational database management systems (RDBMS). This includes fetching, updating, inserting, and removing data from tables.

### **2. What are SQL dialects? Give some examples.**

The various versions of SQL, both free and paid, are also called SQL dialects. All the flavors of SQL have a very similar syntax and vary insignificantly only in additional functionality. Some examples are Microsoft SQL Server, PostgreSQL, MySQL, SQLite, T-SQL, Oracle, and MongoDB.

### **3. What are the main applications of SQL?**

Using SQL, we can:
- create, delete, and update tables in a database
- access, manipulate, and modify data in a table
- retrieve and summarize the necessary information from a table or several tables
- add or remove certain rows or columns from a table

### **4. What is an SQL statement? Give some examples.**

Also known as an SQL command. It's a string of characters interpreted by the SQL engine as a legal command and executed accordingly. Some examples of SQL statements are `SELECT`, `CREATE`, `DELETE`, `DROP`, `REVOKE`, and so on.

### **5. What types of SQL commands (or SQL subsets) do you know?**

- **Data Definition Language (DDL)** – to define and modify the structure of a database.
- **Data Manipulation Language (DML)** – to access, manipulate, and modify data in a database.
- **Data Control Language (DCL)** – to control user access to the data in the database and give or revoke privileges to a specific user or a group of users.
- **Transaction Control Language (TCL)** – to control transactions in a database.
- **Data Query Language (DQL)** – to perform queries on the data in a database to retrieve the necessary information from it.

### **6. Give some examples of common SQL commands of each type.**

- **DDL:** `CREATE`, `ALTER` `TABLE`, `DROP`, `TRUNCATE`, and `ADD COLUMN`
    
- **DML:** `UPDATE`, `DELETE`, and `INSERT`
    
- **DCL:** `GRANT` and `REVOKE`
    
- **TCL:** `COMMIT`, `SET TRANSACTION`, `ROLLBACK`, and `SAVEPOINT`
    
- **DQL:** – `SELECT`

### **7. What is a database?**

A structured storage space where the data is kept in many tables and organized so that the necessary information can be easily fetched, manipulated, and summarized.

### **8. What is DBMS, and what types of DBMS do you know?**

It stands for Database Management System, a software package used to perform various operations on the data stored in a database, such as accessing, updating, wrangling, inserting, and removing data. There are various types of DBMS, such as relational, hierarchical, network, graph, or object-oriented. These types are based on the way the data is organized, structured, and stored in the system.

### **9. What is RDBMS? Give some examples of RDBMS.**

It stands for Relational Database Management System. It's the most common type of DBMS used for working with data stored in multiple tables related to each other by means of shared keys. The SQL programming language is designed to interact with RDBMS. Some examples of RDBMS are MySQL, PostgreSQL, Oracle, MariaDB, etc.

### **10. What are tables and fields in SQL?**

A table is an organized set of related data stored in a tabular form, i.e., in rows and columns. A field is another term for a column of a table.

### **11. What is an SQL query, and what types of queries do you know?**

A query is a piece of code written in SQL to access or modify data from a database.

***There are two types of SQL queries: select and action queries.*** The first ones are used to retrieve the necessary data (this also includes limiting, grouping, ordering the data, extracting the data from multiple tables, etc.), while the second ones are used to create, add, delete, update, rename the data, etc.

### **12. What is a subquery?**

Also called an inner query, a query placed inside another query, or an outer query. A subquery may occur in the clauses such as `SELECT`, `FROM`, `WHERE`, `UPDATE`, etc. It's also possible to have a subquery inside another subquery. The innermost subquery is run first, and its result is passed to the containing query (or subquery).

### **13. What types of SQL subqueries do you know?**

- **Single-row** – returns at most one row.
- **Multi-row** – returns at least two rows.
- **Multi-column** – returns at least two columns.
- **Correlated** – a subquery related to the information from the outer query.
- **Nested** – a subquery inside another subquery.

### **14. What is a constraint, and why use constraints?**

A set of conditions defining the type of data that can be input into each column of a table. ***Constraints ensure data integrity in a table and block undesired actions.***

### **15. What SQL constraints do you know?**

- `DEFAULT` – provides a default value for a column.
    
- `UNIQUE` – allows only unique values.
    
- `NOT NULL` – allows only non-null values.
    
- `PRIMARY KEY` – allows only unique and strictly non-null values (`NOT NULL` and `UNIQUE`).
    
- `FOREIGN KEY` – provides shared keys between two or more tables.
    

### **16. What is a join?**

A clause used to combine and retrieve records from two or multiple tables. SQL tables can be joined based on the relationship between the columns of those tables. Check out our [SQL joins](https://www.datacamp.com/tutorial/introduction-to-sql-joins) [tutorial](https://www.datacamp.com/tutorial/introduction-to-sql-joins) for more context, plus our dedicated guide to [SQL joins interview questions](https://www.datacamp.com/blog/top-sql-joins-interview-questions). 

### **17. What types of joins do you know?**

- `(INNER) JOIN` – returns only those records that satisfy a defined join condition in both (or all) tables. It's a default SQL join.
    
- `LEFT (OUTER) JOIN` – returns all records from the left table and those records from the right table that satisfy a defined join condition.
    
- `RIGHT (OUTER) JOIN` – returns all records from the right table and those records from the left table that satisfy a defined join condition.
    
- `FULL (OUTER) JOIN` – returns all records from both (or all) tables. It can be considered as a combination of left and right joins.
    

### **18. What is a primary key?**

A column (or multiple columns) of a table to which the `PRIMARY KEY` constraint was imposed to ensure unique and non-null values in that column. In other words, a primary key is a combination of the `NOT NULL` and `UNIQUE` constraints. The primary key uniquely identifies each record of the table. Each table should contain a primary key and can't contain more than one primary key.

### **19. What is a unique key?**

A column (or multiple columns) of a table to which the `UNIQUE` constraint was imposed to ensure unique values in that column, including a possible `NULL` value (the only one).

### **20. What is a foreign key?**

A column (or multiple columns) of a table to which the `FOREIGN KEY` constraint was imposed to link this column to the primary key in another table (or several tables). The purpose of foreign keys is to keep connected various tables of a database.

### **21. What is an index?**

A special data structure related to a database table and used for storing its important parts and enabling faster data search and retrieval. Indexes are especially efficient for large databases, where they significantly enhance query performance.

### **22. What types of indexes do you know?**

- **Unique index** – doesn't allow duplicates in a table column and hence helps maintain data integrity.
- **Clustered index** – defines the physical order of records of a database table and performs data searching based on the key values. A table can have only one clustered index.
- **Non-clustered index** – keeps the order of the table records that don't match the physical order of the actual data on the disk. It means that the data is stored in one place and a non-clustered index – in another one. A table can have multiple non-clustered indexes.

### **23. What is a schema?**

A collection of database structural elements such as tables, stored procedures, indexes, functions, and triggers. It shows the overall database architecture, specifies the relationships between various objects of a database, and defines different access permissions for them. Read our [database schema guide](https://www.datacamp.com/tutorial/database-schema) for a deeper understanding.