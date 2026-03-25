## Union Operator

```sql
// returns all rows of SecurityEvent and all rows of SigninLogs

SecurityEvent 
| union SigninLogs  

// returns one row and column, which is the count of all rows of SecurityEvent and all rows of SigninLogs

SecurityEvent 
| union SigninLogs  
| summarize count() 
| project count_

// returns all rows of SecurityEvent and one row for SigninLogs.

SecurityEvent 
| union (SigninLogs | summarize count()| project count_)
```

The union operator supports wildcards to union multiple tables. The following KQL creates a count for the rows in all tables with names that start with Security.

```sql
union Security* 
| summarize count() by Type
```

---
## Join Operator

The join operator **merges the rows of two tables** to form a new table by matching the specified columns' values from each table.

`LeftTable | join [JoinParameters] ( RightTable ) on Attributes`

```sql
SecurityEvent 
| where EventID == "4624" 
| summarize LogOnCount=count() by EventID, Account 
| project LogOnCount, Account 
| join kind = inner (
     SecurityEvent 
     | where EventID == "4634" 
     | summarize LogOffCount=count() by EventID, Account 
     | project LogOffCount, Account 
) on Account
```

>**Task:**  (ChatGPT)
For each **successful sign-in**, if a **process was spawned by the same user** on any endpoint, you’ll get:
>- Username
>- Sign-in timestamp
>- The process they launched

```sql
SigninLogs
| where TimeGenerated >= ago(7d)
| where ResultType == 0
| join kind=inner (
    SecurityEvent
    | where TimeGenerated >= ago(7d)
    | where EventID == 4688
) on $left.UserPrincipalName == $right.Account
| project UserPrincipalName, SignInTime = $left.TimeGenerated, ProcessName = $right.ProcessName
```

![[Pasted image 20250629192752.png | 500]]

|Join Flavor|Output Records|
|---|---|
|kind=leftanti, kind=leftantisemi|Returns all the records from the left side that don't have matches from the right|
|kind=rightanti, kind=rightantisemi|Returns all the records from the right side that don't have matches from the left.|
|kind unspecified, kind=innerunique|Only one row from the left side is matched for each value of the on key. The output contains a row for each match of this row with rows from the right|
|kind=leftsemi|Returns all the records from the left side that have matches from the right.|
|kind=rightsemi|Returns all the records from the right side that have matches from the left.|
|kind=inner|Contains a row in the output for every combination of matching rows from left and right.|
|kind=leftouter (or kind=rightouter or kind=fullouter)|Contains a row for every row on the left and right, even if it has no match. The unmatched output cells contain nulls.|

---

