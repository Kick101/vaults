Kusto Query Language is used to perform **analysis** on data to create:
- analytics
- workbooks
- perform hunting 
in Sentinel and Defender XDR.

> KQL query is a read-only request to process data & return results.

---
## Syntax Structure

```sql
SecurityEvent | where EventID == "4626" | summarize count() by Account | limit 10
```

Similar to this `SQL` query.

```sql
SELECT Account, COUNT(*) 
FROM SecurityEvent WHERE EventID="4626"
GROUP BY Account LIMIT 10;
```

| Part         | Meaning                                                |
| ------------ | ------------------------------------------------------ |
| `summarize`  | `summarize = GROUP BY + AGGREGATE`                     |
| `count()`    | Counts the number of records in each group             |
| `by Account` | Groups the results by the value in the `Account` field |

![[Pasted image 20250629122040.png]]

---

## Search Operator

Although the search operator is easy to use, when compared to the where operator it's inefficient. Even with this inefficiency, you can use search to find data when unsure which table or column to filter.

- The first statement will search for "err" across all tables. 
- The second statement will search for "err" in tables `SecurityEvent`, `SecurityAlert`, and tables starting with A.

```sql
search "err"

search in (SecurityEvent,SecurityAlert,A*) "err"
```

---
## Where Operator

```sql
SecurityEvent
| where TimeGenerated > ago(1d)

SecurityEvent
| where TimeGenerated > ago(1h) and EventID == "4624"

SecurityEvent
| where TimeGenerated > ago(1h)
| where EventID == 4624
| where AccountType =~ "user"

SecurityEvent | where EventID in (4624, 4625)
```

---
## Let Statements

Let statements bind names to expressions. Let statements improve modularity and reuse since they allow you to break a potentially complex expression into multiple parts. Let statements allow for the **creation of user-defined functions and views**. The views are expressions whose results look like a new table.


### Declare and reuse variables

```sql
let timeOffset = 7d;
let discardEventId = 4688;

SecurityEvent
| where TimeGenerated > ago(timeOffset*2) and TimeGenerated < ago(timeOffset)
| where EventID != discardEventId
```

>##### 💡 Tip
`ago()` is a function that takes the current Date and Time and subtract the value provided.

### Declare dynamic tables or lists

**List**

```sql
let suspiciousAccounts = datatable(account: string) [
    @"\administrator", 
    @"NT AUTHORITY\SYSTEM"
];
SecurityEvent | where Account in (suspiciousAccounts)
```

> `@` Prefix in KQL: **Verbatim String Literal**
> It tells KQL to treat the string **exactly as written**, without interpreting escape sequences (like `\`, `\n`, `\t`, etc.).

**Dynamic Table**

```sql
let LowActivityAccounts =
    SecurityEvent 
    | summarize cnt = count() by Account 
    | where cnt < 1000;
LowActivityAccounts | where Account contains "SQL"
```

| Account              | cnt |
| -------------------- | --- |
| `SQLServiceAccount1` | 543 |
| `SQL_Admin`          | 23  |

>You keep only **low-activity accounts**, i.e., accounts that appear fewer than 1000 times in the logs.
>Why? Because accounts with **unusually low activity** might be **dormant accounts that suddenly got used**, which is suspicious.

---
## Extend operator

The extend operator will create calculated columns and append the new columns to the result set.

The KQL example below uses the extend operator to create a new column, **StartDir** containing the directory a process was started in. The **StartDir** column is a calculated column containing the results of a `substring` function.

```sql
SecurityEvent
| where ProcessName != "" and Process != ""
| extend StartDir =  substring(ProcessName,0, string_size(ProcessName)-string_size(Process))
```

>`substring(source_string, start_index, length)`

![[Pasted image 20250629143550.png]]

---
## Order by operator
Sort the rows of the input table by one or more columns.
The default order for a column is descending.

```sql
SecurityEvent
| where ProcessName != "" and Process != ""
| extend StartDir =  substring(ProcessName,0, string_size(ProcessName)-string_size(Process))
| order by StartDir desc, Process asc
```

>For entries with the **same `StartDir`**, it sorts the `Process` names in **ascending (A → Z)** order.

>💡 Bonus: Use `render table` at the end for workbook visuals

---
## project operator
The project operators control what columns to include, add, remove, or rename in the result set of a statement.

```sql
SecurityEvent
| project TimeGenerated, EventID, Account, IpAddress, Computer
| take 20
```

```sql
SecurityEvent
| where ProcessName != "" and Process != ""
| extend StartDir =  substring(ProcessName,0, string_size(ProcessName)-string_size(Process))
| order by StartDir desc, Process asc
| project-away ProcessName
```

> `take` fetches the first records from the table
> `take` and `limit` are synonyms.

|Operator|Description|
|---|---|
|project|Select the columns to include, rename or drop, and insert new computed columns.|
|project-away|Select what columns from the input to exclude from the output.|
|project-keep|Select what columns from the input to keep in the output.|
|project-rename|Select the columns to rename in the resulting output.|
|project-reorder|Set the column order in the resulting output.|

---
# Quick Reference

| Operator/Function                                                                                                  | Description                                                                                                                                                                                                                                                                                             | Syntax                                                                                                                                                                             |
| ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Filter/Search/Condition**                                                                                        | **_Find relevant data by filtering or searching_**                                                                                                                                                                                                                                                      |                                                                                                                                                                                    |
| [where](https://learn.microsoft.com/en-us/kusto/query/where-operator?view=azure-data-explorer)                     | Filters on a specific predicate                                                                                                                                                                                                                                                                         | `T \| where Predicate`                                                                                                                                                             |
| [where contains/has](https://learn.microsoft.com/en-us/kusto/query/where-operator?view=azure-data-explorer)        | `Contains`: Looks for any substring match  <br>`Has`: Looks for a specific word (better performance)                                                                                                                                                                                                    | `T \| where col1 contains/has "[search term]"`                                                                                                                                     |
| [search](https://learn.microsoft.com/en-us/kusto/query/search-operator?view=azure-data-explorer)                   | Searches all columns in the table for the value                                                                                                                                                                                                                                                         | `[TabularSource \|] search [kind=CaseSensitivity] [in (TableSources)] SearchPredicate`                                                                                             |
| [take](https://learn.microsoft.com/en-us/kusto/query/take-operator?view=azure-data-explorer)                       | Returns the specified number of records. Use to test a query  <br>**_Note_**: `take` and `limit` are synonyms.                                                                                                                                                                                          | `T \| take NumberOfRows`                                                                                                                                                           |
| [case](https://learn.microsoft.com/en-us/kusto/query/case-function?view=azure-data-explorer)                       | Adds a condition statement, similar to if/then/elseif in other systems.                                                                                                                                                                                                                                 | `case(predicate_1, then_1, predicate_2, then_2, predicate_3, then_3, else)`                                                                                                        |
| [distinct](https://learn.microsoft.com/en-us/kusto/query/distinct-operator?view=azure-data-explorer)               | Produces a table with the distinct combination of the provided columns of the input table                                                                                                                                                                                                               | `distinct [ColumnName], [ColumnName]`                                                                                                                                              |
| **Date/Time**                                                                                                      | **_Operations that use date and time functions_**                                                                                                                                                                                                                                                       |                                                                                                                                                                                    |
| [ago](https://learn.microsoft.com/en-us/kusto/query/ago-function?view=azure-data-explorer)                         | Returns the time offset relative to the time the query executes. For example, `ago(1h)` is one hour before the current clock's reading.                                                                                                                                                                 | `ago(a_timespan)`                                                                                                                                                                  |
| [format_datetime](https://learn.microsoft.com/en-us/kusto/query/format-datetime-function?view=azure-data-explorer) | Returns data in [various date formats](https://learn.microsoft.com/en-us/kusto/query/format-datetime-function?view=azure-data-explorer#supported-format-elements).                                                                                                                                      | `format_datetime(datetime , format)`                                                                                                                                               |
| [bin](https://learn.microsoft.com/en-us/kusto/query/bin-function?view=azure-data-explorer)                         | Rounds all values in a timeframe and groups them                                                                                                                                                                                                                                                        | `bin(value,roundTo)`                                                                                                                                                               |
| **Create/Remove Columns**                                                                                          | **_Add or remove columns in a table_**                                                                                                                                                                                                                                                                  |                                                                                                                                                                                    |
| [print](https://learn.microsoft.com/en-us/kusto/query/print-operator?view=azure-data-explorer)                     | Outputs a single row with one or more scalar expressions                                                                                                                                                                                                                                                | `print [ColumnName =] ScalarExpression [',' ...]`                                                                                                                                  |
| [project](https://learn.microsoft.com/en-us/kusto/query/project-operator?view=azure-data-explorer)                 | Selects the columns to include in the order specified                                                                                                                                                                                                                                                   | `T \| project ColumnName [= Expression] [, ...]`  <br>Or  <br>`T \| project [ColumnName \| (ColumnName[,]) =] Expression [, ...]`                                                  |
| [project-away](https://learn.microsoft.com/en-us/kusto/query/project-away-operator?view=azure-data-explorer)       | Selects the columns to exclude from the output                                                                                                                                                                                                                                                          | `T \| project-away ColumnNameOrPattern [, ...]`                                                                                                                                    |
| [project-keep](https://learn.microsoft.com/en-us/kusto/query/project-keep-operator?view=azure-data-explorer)       | Selects the columns to keep in the output                                                                                                                                                                                                                                                               | `T \| project-keep ColumnNameOrPattern [, ...]`                                                                                                                                    |
| [project-rename](https://learn.microsoft.com/en-us/kusto/query/project-rename-operator?view=azure-data-explorer)   | Renames columns in the result output                                                                                                                                                                                                                                                                    | `T \| project-rename new_column_name = column_name`                                                                                                                                |
| [project-reorder](https://learn.microsoft.com/en-us/kusto/query/project-reorder-operator?view=azure-data-explorer) | Reorders columns in the result output                                                                                                                                                                                                                                                                   | `T \| project-reorder Col2, Col1, Col* asc`                                                                                                                                        |
| [extend](https://learn.microsoft.com/en-us/kusto/query/extend-operator?view=azure-data-explorer)                   | Creates a calculated column and adds it to the result set                                                                                                                                                                                                                                               | `T \| extend [ColumnName \| (ColumnName[, ...]) =] Expression [, ...]`                                                                                                             |
| **Sort and Aggregate Dataset**                                                                                     | **_Restructure the data by sorting or grouping them in meaningful ways_**                                                                                                                                                                                                                               |                                                                                                                                                                                    |
| [sort operator](https://learn.microsoft.com/en-us/kusto/query/sort-operator?view=azure-data-explorer)              | Sort the rows of the input table by one or more columns in ascending or descending order                                                                                                                                                                                                                | `T \| sort by expression1 [asc\|desc], expression2 [asc\|desc], …`                                                                                                                 |
| [top](https://learn.microsoft.com/en-us/kusto/query/top-operator?view=azure-data-explorer)                         | Returns the first N rows of the dataset when the dataset is sorted using `by`                                                                                                                                                                                                                           | `T \| top numberOfRows by expression [asc\|desc] [nulls first\|last]`                                                                                                              |
| [summarize](https://learn.microsoft.com/en-us/kusto/query/summarize-operator?view=azure-data-explorer)             | Groups the rows according to the `by` group columns, and calculates aggregations over each group                                                                                                                                                                                                        | `T \| summarize [[Column =] Aggregation [, ...]] [by [Column =] GroupExpression [, ...]]`                                                                                          |
| [count](https://learn.microsoft.com/en-us/kusto/query/count-operator?view=azure-data-explorer)                     | Counts records in the input table (for example, T)  <br>This operator is shorthand for `summarize count()`                                                                                                                                                                                              | `T \| count`                                                                                                                                                                       |
| [join](https://learn.microsoft.com/en-us/kusto/query/join-operator?view=azure-data-explorer)                       | Merges the rows of two tables to form a new table by matching values of the specified column(s) from each table. Supports a full range of join types: `fullouter`, `inner`, `innerunique`, `leftanti`, `leftantisemi`, `leftouter`, `leftsemi`, `rightanti`, `rightantisemi`, `rightouter`, `rightsemi` | `LeftTable \| join [JoinParameters] ( RightTable ) on Attributes`                                                                                                                  |
| [union](https://learn.microsoft.com/en-us/kusto/query/union-operator?view=azure-data-explorer)                     | Takes two or more tables and returns all their rows                                                                                                                                                                                                                                                     | `[T1] \| union [T2], [T3], …`                                                                                                                                                      |
| [range](https://learn.microsoft.com/en-us/kusto/query/range-operator?view=azure-data-explorer)                     | Generates a table with an arithmetic series of values                                                                                                                                                                                                                                                   | `range columnName from start to stop step step`                                                                                                                                    |
| **Format Data**                                                                                                    | **_Restructure the data to output in a useful way_**                                                                                                                                                                                                                                                    |                                                                                                                                                                                    |
| [lookup](https://learn.microsoft.com/en-us/kusto/query/lookup-operator?view=azure-data-explorer)                   | Extends the columns of a fact table with values looked-up in a dimension table                                                                                                                                                                                                                          | `T1 \| lookup [kind = (leftouter\|inner)] ( T2 ) on Attributes`                                                                                                                    |
| [mv-expand](https://learn.microsoft.com/en-us/kusto/query/mv-expand-operator?view=azure-data-explorer)             | Turns dynamic arrays into rows (multi-value expansion)                                                                                                                                                                                                                                                  | `T \| mv-expand Column`                                                                                                                                                            |
| [parse](https://learn.microsoft.com/en-us/kusto/query/parse-operator?view=azure-data-explorer)                     | Evaluates a string expression and parses its value into one or more calculated columns. Use for structuring unstructured data.                                                                                                                                                                          | `T \| parse [kind=regex [flags=regex_flags] \|simple\|relaxed] Expression with * (StringConstant ColumnName [: ColumnType]) *...`                                                  |
| [make-series](https://learn.microsoft.com/en-us/kusto/query/make-series-operator?view=azure-data-explorer)         | Creates series of specified aggregated values along a specified axis                                                                                                                                                                                                                                    | `T \| make-series [MakeSeriesParamters] [Column =] Aggregation [default = DefaultValue] [, ...] on AxisColumn from start to end step step [by [Column =] GroupExpression [, ...]]` |
| [let](https://learn.microsoft.com/en-us/kusto/query/let-statement?view=azure-data-explorer)                        | Binds a name to expressions that can refer to its bound value. Values can be lambda expressions to create query-defined functions as part of the query. Use `let` to create expressions over tables whose results look like a new table.                                                                | `let Name = ScalarExpression \| TabularExpression \| FunctionDefinitionExpression`                                                                                                 |
| **General**                                                                                                        | **_Miscellaneous operations and function_**                                                                                                                                                                                                                                                             |                                                                                                                                                                                    |
| [invoke](https://learn.microsoft.com/en-us/kusto/query/invoke-operator?view=azure-data-explorer)                   | Runs the function on the table that it receives as input.                                                                                                                                                                                                                                               | `T \| invoke function([param1, param2])`                                                                                                                                           |
| [evaluate pluginName](https://learn.microsoft.com/en-us/kusto/query/evaluate-operator?view=azure-data-explorer)    | Evaluates query language extensions (plugins)                                                                                                                                                                                                                                                           | `[T \|] evaluate [ evaluateParameters ] PluginName ( [PluginArg1 [, PluginArg2]... )`                                                                                              |
| **Visualization**                                                                                                  | **_Operations that display the data in a graphical format_**                                                                                                                                                                                                                                            |                                                                                                                                                                                    |
| [render](https://learn.microsoft.com/en-us/kusto/query/render-operator?view=azure-data-explorer)                   | Renders results as a graphical output                                                                                                                                                                                                                                                                   | `T \| render Visualization [with (PropertyName = PropertyValue [, ...] )]`                                                                                                         |