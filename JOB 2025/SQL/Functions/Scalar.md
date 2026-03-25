### 🔹 1. **String Functions**

| Function                        | Description                                                                           | Example                            |
| ------------------------------- | ------------------------------------------------------------------------------------- | ---------------------------------- |
| `UPPER(str)`                    | Converts to uppercase                                                                 | `UPPER('sql') → 'SQL'`             |
| `LOWER(str)`                    | Converts to lowercase                                                                 | `LOWER('SQL') → 'sql'`             |
| `LENGTH(str)`                   | Returns length of string                                                              | `LENGTH('SQL') → 3`                |
| `TRIM(str)`                     | Removes leading/trailing spaces                                                       | `TRIM(' SQL ') → 'SQL'`            |
| `SUBSTRING(str, start, length)` | Extracts part of string                                                               | `SUBSTRING('Hello', 2, 3) → 'ell'` |
| `CONCAT(str1, str2)`            | Joins strings                                                                         | `CONCAT('A','B') → 'AB'`           |
| `REPLACE(str, from, to)`        | Replaces substring                                                                    | `REPLACE('abc','a','x') → 'xbc'`   |
| `INSTR(str, substr)`            | Returns position of substring                                                         | `INSTR('abc','b') → 2`             |
| `INITCAP(str)`                  | Converts the first letter of each word to **uppercase** and the rest to **lowercase** | `INITCAP('john doe')`              |

---

### 🔹 2. **Numeric Functions**

|Function|Description|Example|
|---|---|---|
|`ABS(x)`|Absolute value|`ABS(-5) → 5`|
|`CEIL(x)` or `CEILING(x)`|Rounds up|`CEIL(4.3) → 5`|
|`FLOOR(x)`|Rounds down|`FLOOR(4.7) → 4`|
|`ROUND(x, d)`|Rounds to d decimal places|`ROUND(3.14159, 2) → 3.14`|
|`POWER(x, y)`|x to the power y|`POWER(2, 3) → 8`|
|`MOD(x, y)`|Remainder of division|`MOD(10, 3) → 1`|
|`RAND()`|Random float value between 0 and 1|`RAND() → 0.6453`|
|`SQRT(x)`|Square root|`SQRT(9) → 3`|

---

### 🔹 3. **Date & Time Functions**

|Function|Description|Example|
|---|---|---|
|`CURDATE()`|Current date|`2025-05-06`|
|`CURRENT_TIME()`|Current time|`10:30:00`|
|`NOW()` or `CURRENT_TIMESTAMP()`|Current date and time|`2025-05-06 10:30:00`|
|`YEAR(date)`|Extract year|`YEAR('2020-01-01') → 2020`|
|`MONTH(date)`|Extract month|`MONTH('2020-01-01') → 1`|
|`DAY(date)`|Extract day|`DAY('2020-01-01') → 1`|
|`DATEDIFF(date1, date2)`|Difference in days|`DATEDIFF('2025-05-06', '2025-05-01') → 5`|
|`DATE_ADD(date, INTERVAL n DAY)`|Add days|`DATE_ADD('2025-05-01', INTERVAL 5 DAY)`|
|`DATE_FORMAT(date, format)`|Formats a date|`DATE_FORMAT(NOW(), '%Y-%m-%d')`|

---

### 🔹 4. **System / Metadata Functions**

|Function|Description|Example|
|---|---|---|
|`USER()`|Current database user|`user() → 'root@localhost'`|
|`DATABASE()`|Current database|`DATABASE() → 'sales_db'`|
|`VERSION()`|DB version|`VERSION() → '8.0.36'`|
