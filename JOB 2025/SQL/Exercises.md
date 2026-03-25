__2nd Highest Salary__
- First create a result set excluding max(salary) and again fetch max(salary) for second highest.
- First sub-query will be executed.
- DISTINCT - removes duplicates.
- `DISTINCT` applies to **all columns listed in the SELECT** clause.
	- It can add overhead in large datasets because SQL must sort or compare rows to remove duplicates.
	- `DENSE-RANK` (NEED TO BE LOOKED AT)

```sql
SELECT MAX(sal) FROM emp 
WHERE sal NOT IN (SELECT MAX(sal) FROM emp)
```

```SQL
SELECT MAX(sal) FROM emp 
WHERE sal < (SELECT MAX(sal) FROM emp)
```

```SQL
SELECT DISTINCT salary FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 2;
```
---
__Department wise Highest salary__

```SQL
SELECT MAX(sal),dept FROM emp GROUP BY dept
```

- This will give an _error_ because `sal` is not part of an aggregate function or in `GROUP BY`
```SQL
SELECT dept, sal FROM employees GROUP BY dept;
```

- No. of employees from each dept
```SQL
SELECT COUNT(*),dept from emp GROUP BY dept
```

---

__Display Duplicate of a Column__
- WHERE cannot be used with `GROUP BY`
- Because `WHERE` is **processed _before_ grouping happens**.
```SQL
SELECT ename, count(*) FROM emp 
GROUP BY ename
HAVING count (*)>1
```

---
__Display Nth Row__

```SQL
select min(salary) from (select distinct salary from employee 
order by salary desc) 
where rownum <= N;
```

---
__Increase salary by 15% who worked for more than 5 years__
- **`CURDATE() - INTERVAL 5 YEAR`** : Subtracts **5 years** from today’s date
- Example: `'2025-05-05' - INTERVAL 5 YEAR` results in `'2020-05-05'`

```SQL
UPDATE employees
SET salary = salary + salary * 0.15
WHERE department = 'HR'
  AND hire_date < CURDATE() - INTERVAL 5 YEAR;
```
---
