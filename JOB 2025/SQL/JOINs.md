__INNER JOIN__
- Common column data type
- Right table row is joined w/ left table row only if there is a matching for the _left table row in right table row_.
```SQL
SELECT e.name, d.dept_name
FROM employees e, department d
WHERE e.dept_id = d.id
  AND d.dept_name = 'HR';
```
```SQL
SELECT employees.name, departments.dept_name
FROM employees INNER JOIN departments
  ON employees.dept_id = departments.id;
```
- We need `GROUP BY` otherwise all the salaries will be summed from all depts. With group by data is partitioned by dept_name
```SQL
SELECT d.dept_name, SUM(e.sal)
FROM employees e
INNER JOIN department d ON e.dept_id = d.id
GROUP BY d.dept_name;
```
---
__SELF JOIN__
```SQL
SELECT e.name AS employee, m.name AS manager
FROM employees e
JOIN employees m ON e.manager_id = m.id;
```
- Display employee details who are getting more salary than their manager salary
```SQL
SELECT * 
FROM employee e 
JOIN employee m ON e.mgr_id = m.id 
WHERE e.sal > m.sal;
```
- Display the employee details who joined before their manager
```SQL
SELECT e.id, e.name, e.date AS emp_join_date, e.mgr_id
FROM employee e
JOIN employee m ON e.mgr_id = m.id
WHERE e.date < m.date;
```

```SQL
SELECT e1.name AS "emp", e2.name AS "manager"
FROM employees e1, employees e2
WHERE e1.mgr = e2.empno;
```

```SQL
SELECT e1.name AS emp, e2.name AS manager
FROM employees e1
JOIN employees e2 ON e1.mgr = e2.empno;
```
---
__LEFT JOIN__
- A **LEFT JOIN** returns **all rows** from the **left table** (the table before the `JOIN`), and the matching rows from the **right table** (the table after the `JOIN`). 
- If there is no match, the result is `NULL` for columns from the right table.
- `WHERE` filters out `NULL` values 

```SQL
SELECT e.name, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id;
```
---
__RIGHT JOIN__
```SQL
SELECT columns
FROM left_table
RIGHT JOIN right_table
ON left_table.column = right_table.column;
```
---
__FULL JOIN__
- MySQL does **not** support `FULL JOIN` (or `FULL OUTER JOIN`) directly.
- But you can **simulate it** using a combination of `LEFT JOIN` and `RIGHT JOIN` with `UNION`.
- Supported:
	- PostgreSQL
	- SQL Server
	- Oracle
	- Snowflake
	- BigQuery
```SQL
SELECT c.name, o.product 
FROM customers c 
FULL JOIN orders o 
ON o.customer_id = c.id;
```

```SQL
SELECT c.name, o.product 
FROM customers c 
LEFT JOIN orders o ON c.id = o.customer_id

UNION

SELECT c.name, o.product 
FROM customers c 
RIGHT JOIN orders o ON c.id = o.customer_id;
```
---
__CROSS JOIN__
- No condition is applied.
- It **does not filter** the results. Instead, it combines every row from `table1` with every row from `table2`.
- The total number of rows in the result will be:  
    **`Number of rows in table1 * Number of rows in table2`**

```SQL
SELECT s.name, c.course_name
FROM students s
CROSS JOIN courses c;
```
---
### 🧠 Summary

| Join Type      | Returns...                                                          | When to Use                                                   | MySQL Support                |
| -------------- | ------------------------------------------------------------------- | ------------------------------------------------------------- | ---------------------------- |
| **INNER JOIN** | Only rows with **matching values** in both tables                   | When you want matched data only                               | ✅ Yes                        |
| **LEFT JOIN**  | All rows from **left table** + matched rows from right, else `NULL` | Show all from left, even if no match                          | ✅ Yes                        |
| **RIGHT JOIN** | All rows from **right table** + matched rows from left, else `NULL` | Show all from right, even if no match                         | ✅ Yes                        |
| **FULL JOIN**  | All rows from **both tables**, with `NULL` where no match           | Complete view of both sides, matched or not                   | ❌ No (simulate with `UNION`) |
| **CROSS JOIN** | **Cartesian product** (every row from A with every row from B)      | Combinations, pairing everything with everything              | ✅ Yes                        |
| **SELF JOIN**  | A table joined with **itself**                                      | Compare rows in the same table (e.g., employees and managers) | ✅ Yes                        |
