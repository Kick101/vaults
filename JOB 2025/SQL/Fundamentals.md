### SQL Clause Execution Order
```SQL
SELECT ...
FROM ...
WHERE ...
GROUP BY ...
HAVING ...
ORDER BY ...
```

---
### Data Types

|**Category**|**Data Type**|**Description**|**Example**|**Range/Size**|
|---|---|---|---|---|
|**Numeric Types**|**`TINYINT`**|Stores small integers.|`TINYINT(3)`|-128 to 127|
||**`SMALLINT`**|Stores small integers.|`SMALLINT`|-32,768 to 32,767|
||**`MEDIUMINT`**|Stores medium-sized integers.|`MEDIUMINT`|-8,388,608 to 8,388,607|
||**`INT`**|Stores standard-sized integers.|`INT`|-2,147,483,648 to 2,147,483,647|
||**`BIGINT`**|Stores large integers.|`BIGINT`|-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807|
||**`FLOAT`**|Stores single-precision floating-point numbers.|`FLOAT(7,4)`|-3.402823466E+38 to 3.402823466E+38|
||**`DOUBLE`**|Stores double-precision floating-point numbers.|`DOUBLE`|-1.7976931348623157E+308 to 1.7976931348623157E+308|
||**`DECIMAL(M,D)`**|Stores exact numbers with specified precision (useful for currency).|`DECIMAL(10, 2)`|Up to 10 digits, 2 after decimal|
|**Character Types**|**`CHAR(M)`**|Fixed-length string.|`CHAR(10)`|Exactly 10 characters|
||**`VARCHAR(M)`**|Variable-length string.|`VARCHAR(100)`|Up to 255 characters|
|**Text Types**|**`TEXT`**|Stores long text (e.g., descriptions).|`TEXT`|Up to 65,535 characters|
||**`TINYTEXT`**|Stores very short text.|`TINYTEXT`|Up to 255 characters|
||**`MEDIUMTEXT`**|Stores medium-length text.|`MEDIUMTEXT`|Up to 16,777,215 characters|
||**`LONGTEXT`**|Stores very large text.|`LONGTEXT`|Up to 4,294,967,295 characters|
|**Date and Time Types**|**`DATE`**|Stores date values (year, month, day).|`DATE`|`YYYY-MM-DD`|
||**`DATETIME`**|Stores both date and time values.|`DATETIME`|`YYYY-MM-DD HH:MM:SS`|
||**`TIMESTAMP`**|Stores both date and time; automatically updated when a record is modified.|`TIMESTAMP`|`YYYY-MM-DD HH:MM:SS`|
||**`TIME`**|Stores only time values (hours, minutes, seconds).|`TIME`|`HH:MM:SS`|
||**`YEAR`**|Stores a 4-digit year value.|`YEAR`|`YYYY`|
|**Binary Types**|**`BINARY(M)`**|Stores fixed-length binary data.|`BINARY(16)`|16 bytes|
||**`VARBINARY(M)`**|Stores variable-length binary data.|`VARBINARY(255)`|255 bytes|
||**`BLOB`**|Stores large binary data (e.g., images).|`BLOB`|Up to 65,535 bytes|
||**`TINYBLOB`**|Stores small binary data.|`TINYBLOB`|Up to 255 bytes|
||**`MEDIUMBLOB`**|Stores medium-sized binary data.|`MEDIUMBLOB`|Up to 16,777,215 bytes|
||**`LONGBLOB`**|Stores very large binary data.|`LONGBLOB`|Up to 4,294,967,295 bytes|
|**Miscellaneous Types**|**`ENUM`**|Stores a predefined list of values.|`ENUM('small', 'medium', 'large')`|1 value from the list|
||**`SET`**|Stores a set of predefined values (can store multiple values).|`SET('A', 'B', 'C')`|One or more values from the set|
|**JSON Type**|**`JSON`**|Stores JSON data, useful for unstructured data storage.|`JSON`|Varies based on the content (up to several MBs)|

---
### 🏗️ **1. DDL (Data Definition Language)**

**DDL** commands are used to define or manage the structure of a database, including 
- tables
- indexes
- constraints. 

>These commands deal with **schema** changes, i.e., the structure, but **not the data** itself.
#### 1. **CREATE**

Creates new database objects (tables, views, indexes, or databases).

**Create Table:**

```sql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  salary DECIMAL(10, 2),
  hire_date DATE
);
```

 **Create Index:**   
- 
```sql
CREATE INDEX idx_emp_name ON employees (name);
```

**Create Database:**

```sql
CREATE DATABASE company;
```

#### 2. **ALTER**

Modifies an existing database object (table or column).

**Add Column to Table:**
```sql
ALTER TABLE employees ADD email VARCHAR(100);
```

**Modify Column Data Type:**

```sql
ALTER TABLE employees MODIFY salary DECIMAL(12, 2);
```

**Drop Column from Table:**    

```sql
ALTER TABLE employees DROP COLUMN email;
```


#### 3. **DROP**

Removes an existing database object(table, index, view, etc.). 

>**All data in the object is deleted** as well.

**Drop Table:**

```sql
DROP TABLE employees;
```

 **Drop Index:**

```sql
DROP INDEX idx_emp_name ON employees;
```

 **Drop Database:**

```sql
DROP DATABASE company;
```


#### 4. **TRUNCATE**

Removes all data from a _table_, but **keeps the table structure** intact. 
>It is **faster** than `DELETE` because it does not log individual row deletions.

- **Truncate Table:**

```sql
TRUNCATE TABLE employees;
```


#### 5. **RENAME**

The `RENAME` command is used to **change the name of a table or column**.

- **Rename Table:**

```sql
RENAME TABLE employees TO staff;
```

- **Rename Column:**  
    (Note: RENAME COLUMN support may vary by RDBMS)

```sql
ALTER TABLE employees RENAME COLUMN name TO full_name;
```


#### 6. **COMMENT**

```sql
COMMENT ON COLUMN employees.salary IS 'Employee salary in USD';
```

#### Summary of Common DDL Commands:

| Command    | Description                                    | Example Query                                                     |
| ---------- | ---------------------------------------------- | ----------------------------------------------------------------- |
| `CREATE`   | Creates a new database object                  | `CREATE TABLE employees (...);`                                   |
| `ALTER`    | Modifies an existing database object           | `ALTER TABLE employees ADD email VARCHAR(100);`                   |
| `DROP`     | Deletes a database object (table, index, etc.) | `DROP TABLE employees;`                                           |
| `TRUNCATE` | Removes all rows from a table, keeps structure | `TRUNCATE TABLE employees;`                                       |
| `RENAME`   | Renames a database object                      | `RENAME TABLE employees TO staff;`                                |
| `COMMENT`  | Adds comments to describe a database object    | `COMMENT ON COLUMN employees.salary IS 'Employee salary in USD';` |

#### 🔧 **Note on Transaction Control**:

>Although `CREATE`, `ALTER`, `DROP`, and `TRUNCATE` are DDL commands, they typically **cannot** be rolled back in most databases. However, `ALTER` and `TRUNCATE` are **transaction-safe** in some systems (like MySQL), but **`DROP` and `CREATE`** usually **commit automatically** and cannot be rolled back without a transaction.

---
### 📝 **2. DML (Data Manipulation Language)**

**DML** is a category of SQL commands used to **manipulate the data** stored in database tables — 
- inserting
- updating
- deleting
- retrieving

|Command|Description|
|---|---|
|`SELECT`|Retrieve data from tables|
|`INSERT`|Add new rows of data into a table|
|`UPDATE`|Modify existing data in a table|
|`DELETE`|Remove existing rows from a table|
|`MERGE`|Combine `INSERT`, `UPDATE`, and `DELETE` logic|

#### 🔍 **1. SELECT** – Read data from a table

```sql
SELECT * FROM employees;
SELECT name, salary FROM employees WHERE department = 'HR';
```


#### ➕ **2. INSERT** – Add data into a table

__Insert a single row__

```sql
INSERT INTO employees (id, name, salary, department)
VALUES (1, 'Alice', 5000, 'HR');
```

__Insert multiple rows__

```sql
INSERT INTO employees (id, name, salary, department)
VALUES 
(2, 'Bob', 4500, 'IT'),
(3, 'Charlie', 5200, 'Sales');
```


#### ✏️ **3. UPDATE** – Modify existing data

```sql
UPDATE employees
SET salary = salary + 500
WHERE department = 'Sales';
```

> ⚠️ If `WHERE` is omitted, **all rows will be updated**.


#### ❌ **4. DELETE** – Remove rows

```sql
DELETE FROM employees
WHERE department = 'HR';
```

> ⚠️ If `WHERE` is omitted, **all rows will be deleted**.


#### 🔁 **5. MERGE (also called UPSERT)** – Insert or update in one step

(Supported in SQL Server, Oracle, and PostgreSQL with `ON CONFLICT`)

#### Example (Oracle):

```sql
MERGE INTO employees e
USING new_employees n
ON (e.id = n.id)
WHEN MATCHED THEN
  UPDATE SET e.salary = n.salary
WHEN NOT MATCHED THEN
  INSERT (id, name, salary)
  VALUES (n.id, n.name, n.salary);
```


#### 🔐 DML & Transactions

All DML operations can be part of a **transaction**:

```sql
BEGIN;

UPDATE employees SET salary = salary + 1000 WHERE id = 1;
DELETE FROM employees WHERE id = 99;

COMMIT; -- or ROLLBACK;
```


#### Summary:

|DML Command|Action|
|---|---|
|`SELECT`|Reads data from tables|
|`INSERT`|Adds new rows|
|`UPDATE`|Changes existing row values|
|`DELETE`|Removes rows|
|`MERGE`|Conditionally inserts or updates rows|

---

### 🔐 **3. DCL (Data Control Language)**

**DCL (Data Control Language)** is a subset of SQL used for controlling access to data and managing permissions in a database. It deals with granting and revoking permissions for users. 

#### **1. GRANT**

The `GRANT` statement is used to provide specific privileges to users or roles on database objects (such as tables, views, etc.).

**Syntax:**

```sql
GRANT privilege_type ON object TO user_or_role;
```

**Example:**

```sql
GRANT SELECT, INSERT ON employees TO 'john_doe';
```


You can also grant privileges to roles, so multiple users in that role can have the same privileges.

```sql
GRANT SELECT ON employees TO hr_role;
```

#### **2. REVOKE**

The `REVOKE` statement is used to remove or withdraw specific privileges from users or roles.

**Syntax:**

```sql
REVOKE privilege_type ON object FROM user_or_role;
```

**Example:**

```sql
REVOKE SELECT, INSERT ON employees FROM 'john_doe';
```


#### DCL Permissions Overview

__Common privileges:__
- **SELECT**: Allows a user to query data from a table.
- **INSERT**: Allows a user to insert data into a table.
- **UPDATE**: Allows a user to update existing data in a table.
- **DELETE**: Allows a user to delete data from a table.
- **ALL**: Grants all privileges (like SELECT, INSERT, UPDATE, DELETE, etc.) on a table.
- **EXECUTE**: Allows a user to execute a stored procedure or function.

#### Example of Granting All Privileges

```sql
GRANT ALL PRIVILEGES ON employees TO 'manager';
```

#### Example of Revoking All Privileges

```sql
REVOKE ALL PRIVILEGES ON employees FROM 'manager';
```

#### Additional Points
- **WITH GRANT OPTION**: When granted permissions with this option, the user can also grant those same privileges to other users.

```sql
GRANT SELECT ON employees TO 'manager' WITH GRANT OPTION;
```
- This allows `manager` to not only perform `SELECT` operations but also grant `SELECT` privileges to other users.
- **Database-wide Permissions**: You can grant permissions on entire databases as well as individual tables or views.

```sql
GRANT ALL PRIVILEGES ON database_name.* TO 'user';
```

#### Role-Based Access Control (RBAC)

Instead of granting privileges directly to users, you can create roles and grant privileges to the roles. Users can then be assigned to roles.

**Example:**

```sql
CREATE ROLE admin_role;
GRANT SELECT, INSERT, UPDATE ON employees TO admin_role;
```

Then assign the role to users:

```sql
GRANT admin_role TO 'john_doe';
```

#### Managing Permissions Example

1. **Grant permissions:**
```sql
GRANT SELECT, INSERT ON employees TO 'alice';
```
2. **Grant role permissions:**
```sql
CREATE ROLE hr_role;
GRANT SELECT, INSERT, UPDATE ON employees TO hr_role;
```
3. **Revoke permissions:**
```sql
REVOKE SELECT, INSERT ON employees FROM 'alice';
```
4. **Revoke role-based permissions:**
```sql
REVOKE hr_role FROM 'alice';
```

#### Why DCL is Important?
DCL plays an important role in database security
- **Ensures data confidentiality**: By controlling which users can view or manipulate the data.
- **Prevents unauthorized access**: By limiting what actions can be performed on the database objects.
---
### 🔁 **4. TCL (Transaction Control Language)**

**TCL (Transaction Control Language)** is a subset of SQL used to manage transactions in a database. Transactions are sequences of one or more SQL operations that are treated as a single unit of work. TCL ensures **data integrity and consistency** by allowing control over when changes made by DML statements (like `INSERT`, `UPDATE`, `DELETE`) are saved to the database.

| Command           | Description                                                          |
| ----------------- | -------------------------------------------------------------------- |
| `COMMIT`          | Saves all changes made in the current transaction permanently.       |
| `ROLLBACK`        | Undoes all changes made in the current transaction.                  |
| `SAVEPOINT`       | Sets a point within a transaction to which you can later roll back.  |
| `SET TRANSACTION` | Specifies characteristics of the transaction (like isolation level). |

#### 🔹 1. `COMMIT`

**Purpose**: Ends the current transaction and makes all changes made by the transaction permanent.

**Syntax:**

```sql
COMMIT;
```

**Example:**

```sql
UPDATE employees SET salary = salary + 1000 WHERE department = 'IT';
COMMIT;
```

#### 🔹 2. `ROLLBACK`

**Purpose**: Undoes all changes made in the current transaction since the last `COMMIT` or `SAVEPOINT`.

**Syntax:**

```sql
ROLLBACK;
```

**Example:**

```sql
DELETE FROM employees WHERE department = 'HR';
ROLLBACK; -- Undoes the delete
```

#### 🔹 3. `SAVEPOINT`

**Purpose**: Sets a savepoint _within a transaction_ so that you can roll back to that point without rolling back the entire transaction.

**Syntax:**

```sql
SAVEPOINT savepoint_name;
```

**Example:**

```sql
BEGIN;

UPDATE employees SET salary = salary + 500 WHERE department = 'Sales';
SAVEPOINT after_sales_update;

UPDATE employees SET salary = salary + 700 WHERE department = 'HR';
ROLLBACK TO after_sales_update;

COMMIT;
```

#### 🔹 4. `SET TRANSACTION`

**Purpose**: Sets the characteristics of the current transaction, like isolation level (to control concurrency issues).

**Syntax:**

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```


#### 💡 **Key Points about TCL:**

- TCL commands **cannot be used on DDL** (like `CREATE`, `ALTER`, `DROP`). DDL commands auto-commit.
- If **auto-commit is ON**, each SQL statement is automatically committed right after it's executed.
- You must use `BEGIN` (or `START TRANSACTION` in MySQL) to start a manual transaction when auto-commit is off.

#### ✅ Use Case Example (MySQL-style):

```sql
START TRANSACTION;

INSERT INTO employees(name, department, salary) VALUES ('Alice', 'IT', 6000);
UPDATE employees SET salary = salary + 500 WHERE name = 'Bob';

SAVEPOINT after_update;

DELETE FROM employees WHERE name = 'Charlie';

ROLLBACK TO after_update;

COMMIT;
```


>- **SQL `COMMIT`** = apply and _lose the chance to undo_.
>- **Git `commit`** = save a version you _can always go back to_.

---
## 🧠 Summary Table:  DDL, DML, DCL, TCL

| Type | Purpose             | Examples                       |
| ---- | ------------------- | ------------------------------ |
| DDL  | Define structure    | CREATE, ALTER, DROP            |
| DML  | Manage data         | SELECT, INSERT, UPDATE, DELETE |
| DCL  | Manage permissions  | GRANT, REVOKE                  |
| TCL  | Manage transactions | COMMIT, ROLLBACK, SAVEPOINT    |

---
### Having v/s WHERE
| Feature                   | `WHERE`                                        | `HAVING`                                          |
| ------------------------- | ---------------------------------------------- | ------------------------------------------------- |
| **Purpose**               | Filters rows **before** grouping/aggregation   | Filters groups **after** aggregation              |
| **Works with Aggregates** | ❌ No (you can’t use `SUM()`, `COUNT()` etc.)   | ✅ Yes (used to filter by aggregates)              |
| **Used With**             | All SELECT queries                             | Usually used **with `GROUP BY`** (but not always) |
| **Execution Order**       | Executed **before** grouping                   | Executed **after** grouping                       |
| **Can Use Aliases**       | ❌ No                                           | ✅ Yes (from SELECT)                               |
| **Typical Use Case**      | Filter specific rows (e.g., department = 'HR') | Filter grouped results (e.g., COUNT > 5)          |
```SQL
SELECT SUM(sal) FROM emp WHERE name LIKE "j%"; -- correct
```
- Wrong: using `HAVING` with a non-aggregate column (name)
```SQL
SELECT SUM(sal) FROM emp HAVING name LIKE 'j%'; -- Wrong
```

```SQL
SELECT department, SUM(sal) AS total_salary FROM emp GROUP BY department HAVING SUM(sal) > 10000; -- Correct
```
---
