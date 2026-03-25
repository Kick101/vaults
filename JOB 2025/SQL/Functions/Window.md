
| Feature               | `ROWNUM`                                            | `ROW_NUMBER()`                                                    |
| --------------------- | --------------------------------------------------- | ----------------------------------------------------------------- |
| **Function Type**     | Pseudo-column (Oracle-specific)                     | Window function (Standard SQL, used in many RDBMS)                |
| **Assignment Timing** | Assigned **before** sorting                         | Assigned **after** sorting                                        |
| **Sorting**           | **Cannot be used** with `ORDER BY` directly         | Works with `ORDER BY` to assign numbers based on sorted order     |
| **Usage**             | Often used for **pagination** and **limiting rows** | Used for **sorting**, **pagination**, and **partitioning**        |
| **Flexibility**       | Limited to sequential numbering without sorting     | Supports **partitioning** and can reset row numbers within groups |

__ROW_NUMBER, RANK, DENSE_RANK__

| Function       | Gaps in Ranking | Use Case                                         |
| -------------- | --------------- | ------------------------------------------------ |
| `ROW_NUMBER()` | ❌ No gaps       | Always gives unique numbers (1, 2, 3...)         |
| `RANK()`       | ✅ Gaps possible | Gives the same rank for ties, then skips numbers |
| `DENSE_RANK()` | ❌ No gaps       | Same rank for ties, but **no skipping**          |
 
>- The query is wrapped in a subquery (`ranked_orders`) because `WHERE` clause is processed **before** the window functions.
> - When you wrap a `SELECT` statement inside parentheses (i.e., a **subquery**), SQL requires you to give it a name (an alias), because that subquery becomes a **derived table**. SQL needs a name to refer to it — even if you’re not explicitly using it.

---
### ROWNUM & ROW_NUMBER()
 - `rownum` - pseudo column has no storage and _read only_. (Oracle based)
 - `*` cannot be used with pseudo columns.

```SQL
SELECT * 
FROM (SELECT empno, ename, sal, rownum rn FROM emp ORDER BY rn)
WHERE mod (rn, 2) = 0; -- Evene records
```

- `OVER()` _must_ have an **`ORDER BY` clause**, not just a column name.
```SQL
SELECT empno, ename, sal, ROW_NUMBER() OVER (ORDER BY empno) AS row_num
FROM emp;
```


```SQL
SELECT *
FROM (
	SELECT *, ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date) AS order_rnk FROM orders 
	) AS ranked_orders WHERE order_rnk = 1;

```
