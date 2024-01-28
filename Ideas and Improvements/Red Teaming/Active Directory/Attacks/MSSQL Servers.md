- SQL Servers provide very good options for _lateral movement_ as domain users can be mapped to database roles.
- For MSSQL and PowerShell hackery, lets use [PowerUpSQL](https://github.com/NetSPI/PowerUpSQL)
- HeidiSQL is a GUI Tool

#### Enumeration
- Discovery (SPN Scanning)
```powershell
Get-SQLInstanceDomain
```

-  Check Accessibility
```powershell
Get-SQLConnectionTestThreaded
```
```powershell
Get-SQLInstanceDomain | Get-SQLConnectionTestThreaded -Verbose
```
- Gather Information
```powershell
Get-SQLInstanceDomain | Get-SQLServerInfo -Verbose
```

#### Database Links
- A database link allows a SQL Server to access external data sources like other SQL Servers and OLE DB data sources.
- In case of database links between SQL servers, that is, linked SQL servers, it is possible to execute stored procedures.
- _Database links work even across forest trusts._

__Enumerate DB Link__
```powershell
Get-SQLServerLink -instance dcorp-mssql -Verbose
```
__Access Linked DB if `is_data_access_enabled: true`__
```sql
select * from master..sysservers
```
__Enumerating Database Links - Manually__
- Openquery() function can be used to run queries on a linked DB (dcorp-sql1)
```sql
select * from openquery("dcorp-sql1",'select * from master..sysservers')
```
__Enumerating Database Links - Recursively__
```powershell
Get-SQLServerLinkCrawl -Instance dcorp-mssql -Verbose
```

 

