### 📊 Aggregation Functions ?

**Aggregation functions** perform a calculation on a set of values and return a single value — often used with `GROUP BY`.

---

### ✅ Common SQL Aggregate Functions

|Function|Description|Example Use|
|---|---|---|
|`COUNT()`|Counts rows|`COUNT(*)` or `COUNT(column)`|
|`SUM()`|Adds numeric values|`SUM(salary)`|
|`AVG()`|Calculates average|`AVG(score)`|
|`MIN()`|Finds the minimum value|`MIN(age)`|
|`MAX()`|Finds the maximum value|`MAX(price)`|

>- `GROUP BY` - groups rows **based on one or more columns** by their values.
>- No other columns should be used with `GROUP BY` other than aggregating column (MAX()) and group by column itself.

---
### 🔍 **Example Queries**

1. **Total salary of all employees**

```sql
SELECT SUM(salary) AS total_salary FROM employees;
```

2. **Average salary in each department**

```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department;
```

3. **Department with maximum salary**

```sql
SELECT department, MAX(salary) AS max_salary
FROM employees
GROUP BY department;
```

4. **Count of employees per department**

```sql
SELECT department, COUNT(*) AS total_employees
FROM employees
GROUP BY department;
```

