### Pattern Matching
- Display name starting w/ "M"
- Display name ends w/ "M"
- Display name w/ "M" in any position
- Display names without "M" in them
```SQL
SELECT ename FROM emp WHERE ename like "M%"
```
```SQL
SELECT ename FROM emp WHERE ename like "%M"
```
```SQL
SELECT ename FROM emp WHERE ename like "%M%"
```
```SQL
SELECT ename FROM emp WHERE ename NOT LIKE "%M%"
```
---
### Pattern Searching
- Names with 4 letters
- Names with "L" as 2nd letter & 'M' as 4th letter
- Names & dates -- joined in the Dec
- Names with 2 "L's"
- Names start 'j' & end with 's'
- `%` - 0 or more characters.
```SQL
SELECT ename FROM emp WHERE ename LIKE '____';
```
```SQL
SELECT ename FROM emp WHERE ename LIKE '_L_M%';
```
```SQL
SELECT ename,join_date FROM emp WHERE join_date LIKE '%DEC%';
```
- MYSQL function: `MONTHNAME()` (YYYY-MM-DD)
```SQL
SELECT ename, join_date FROM emp WHERE MONTHNAME(join_date) = 'December';
```
```SQL
SELECT ename FROM emp WHERE ename LIKE '%L%L%';
```
```SQL
SELECT ename FROM emp WHERE ename LIKE 'J%S'
```