Write a query to detect accounts that **signed in from more than 3 distinct IP addresses** in a **single day**, over the last 7 days.

```sql
SigninLogs
| where TimeGenerated >= ago(7d)                         
| where ResultType == 0                                   
| summarize ipcnt = dcount(IpAddress) 
    by UserPrincipalName, bin(TimeGenerated, 1d)          
| where ipcnt > 3                                      
```

Write a query to detect accounts that **signed in from more than 3 distinct IP addresses** in a **single day**, over the last 7 days.

```sql
SigninLogs 
| where TimeGenerated >= ago(3d) 
| summarize IPList = make_set(IpAddress) by UserPrincipalName
| project UserPrincipalName, IPList
```

Write a KQL query that shows accounts that:
- Had a successful sign-in **in the last 3 days**
- AND
- Triggered an alert from Microsoft Defender for Identity

```sql
let signins = SigninLogs
| where TimeGenerated >= ago(3d)
| where ResultType == 0
| project SigninTime = TimeGenerated, UserPrincipalName;

let alerts = SecurityAlert
| where TimeGenerated >= ago(3d)
| mv-expand Entity = Entities
| extend entity = parse_json(Entity)
| where entity.Type == "account"
| extend AccountName = tostring(entity.Name)
| project AlertTime = TimeGenerated, AccountName, AlertName;

signins
| join kind=inner (alerts) on $left.UserPrincipalName == $right.AccountName
| project UserPrincipalName, SigninTime, AlertTime, AlertName
```




