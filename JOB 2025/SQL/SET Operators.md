### 📌 Set Operators

Set operators combine results from **two or more `SELECT` queries** and return a single result set.

---
### 🔗 **Common Set Operators:**

|Operator|Description|
|---|---|
|`UNION`|Combines rows from both queries, removes duplicates|
|`UNION ALL`|Combines rows from both queries, includes duplicates|
|`INTERSECT`|Returns **only the rows common** to both queries|
|`MINUS` / `EXCEPT`|Returns rows from the first query **not found** in the second|

---
### INTERSECT

```sql
SELECT name FROM students
INTERSECT
SELECT name FROM honor_roll;
```

→ Returns students **who are also on the honor roll**

---
### MINUS /  EXCEPT

```sql
SELECT name FROM students
MINUS
SELECT name FROM honor_roll;
```

→ Returns students **not on the honor roll**

---
###  UNION
- Same no.of columns and same data type from both the tables
- `UNION ALL` - Duplicates are not eliminated.
```SQL
SELECT * FROM emp0 UNION SELECT * FROM emp2
```

---
### ⚠️ Notes:

- `INTERSECT` and `MINUS` are **not supported in MySQL**, but they work in **Oracle** and **PostgreSQL**.
    
- In MySQL, similar behavior can be achieved using `INNER JOIN`, `LEFT JOIN`, or `NOT EXISTS`.
    
