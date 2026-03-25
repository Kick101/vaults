## summarize operator

The following example returns a count of unique IP Addresses.

```sql
SecurityEvent
| summarize dcount(IpAddress)
```

**Detect Invalid Password failures across multiple applications for the same account (Brute-force)**

`summarize` produces a distinct count of application names and **group by User and IP Address.** Finally, there's a check against a variable created (threshold) to see if the number exceeds the allowed amount.

```sql
let timeframe = 30d;
let threshold = 1;
SigninLogs
| where TimeGenerated >= ago(timeframe)
| where ResultDescription has "Invalid password"
| summarize applicationCount = dcount(AppDisplayName) by UserPrincipalName, IPAddress
| where applicationCount >= threshold
```

**Example Output:**

|UserPrincipalName|AppDisplayName|IPAddress|Result|
|---|---|---|---|
|[alice@contoso.com](mailto:alice@contoso.com)|Microsoft Teams|203.0.113.5|Invalid Password|
|[alice@contoso.com](mailto:alice@contoso.com)|Azure Portal|203.0.113.5|Invalid Password|
|[alice@contoso.com](mailto:alice@contoso.com)|SharePoint Online|203.0.113.5|Invalid Password|

|Function(s)|Description|
|---|---|
|count(), countif()|Returns a count of the records per summarization group|
|dcount(), dcountif()|Returns an estimate for the number of distinct values taken by a scalar expression in the summary group.|
|avg(), avgif()|Calculates the average of Expr across the group.|
|max(), maxif()|Returns the maximum value across the group.|
|min(), minif()|Returns the minimum value across the group.|
|percentile()|Returns an estimate for the specified nearest-rank percentile of the population defined by _Expr_. The accuracy depends on the density of population in the region of the percentile.|
|stdev(), stdevif()|Calculates the standard deviation of Expr across the group, considering the group as a sample.|
|sum(), sumif()|Calculates the sum of Expr across the group.|
|variance(), varianceif()|Calculates the variance of Expr across the group, considering the group as a sample.|

---
## summarize operator to filter results

The `arg_max()` and `arg_min()` functions filter out **top and bottom rows** respectively.

The following statement returns the **most current row** from the `SecurityEvent` table for the computer `SQL10.NA.contosohotels.com`. The `*` in the `arg_max` function requests all columns for the row.

```sql
SecurityEvent 
| where Computer == "SQL10.na.contosohotels.com"
| summarize arg_max(TimeGenerated,*) by Computer
```

In this statement, the oldest `SecurityEvent` for the computer `SQL10.NA.contosohotels.com` is returned as the result set.

```sql
SecurityEvent 
| where Computer == "SQL10.na.contosohotels.com"
| summarize arg_min(TimeGenerated,*) by Computer
```

---
## summarize operator to prepare data

The `make_` functions return a dynamic (**JSON**) array based on the specific function's purpose.

### make_list() function

for each Computer, the results are a JSON array of Accounts. The resulting JSON array will include **duplicate** accounts.

```sql
SecurityEvent
| where EventID == "4624"
| summarize make_list(Account) by Computer
```

### make_set() function

for each Computer, the results are a JSON array of **unique** Accounts.

```sql
SecurityEvent
| where EventID == "4624"
| summarize make_set(Account) by Computer
```

---
## render operator

The render operator generates a visualization of the query results.

The supported visualizations are:

- `areachart`
- `barchart`
- `columnchart`
- `piechart`
- `scatterchart`
- `timechart`

```sql
SecurityEvent 
| summarize count() by Account
| render barchart
```

### Use the summarize operator to create time series

The `bin()` function rounds values down to an integer multiple of the given bin size. Used frequently in combination with summarize by .... If you have a scattered set of values, the values are grouped into a smaller set of specific values. Combining the generated time series and pipe to a render operator with a type of **timechart** provides a time-series visualization.

```sql
SecurityEvent 
| summarize count() by bin(TimeGenerated, 1d) 
| render timechart
```

>`bin(TimeGenerated, 1d)` **rounds** the `TimeGenerated` to the **nearest day**.
>`bin(column, bucket_size)`

```sql
SecurityEvent
| summarize count() by bin(TimeGenerated, 1h)
```

📌 _Counts how many events happened in each hour._

```sql
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| summarize count() by bin(CounterValue, 10)
```

📌 _Groups CPU data into: 0–9%, 10–19%, 20–29%, ..._


