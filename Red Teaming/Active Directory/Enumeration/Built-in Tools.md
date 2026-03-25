___Enumeration List___
-   The domain functional level
-   The domain password policy
-   A full inventory of AD users
-   A full inventory of AD computers
-   A full inventory of AD groups and memberships
-   Domain trust relationships
-   Object ACLs
-   Group Policy Objects (GPO) information
-   Remote access rights

__RDP Connection__
```bash
xfreerdp /v:$IP /u:htb-student /p:<password> /cert-ignore
```
---
### RSAT
>Remote Server Administration Tools allows systems administrators to remotely manage Windows Server roles and features

__Install using script:__ [script](https://gist.github.com/dually8/558fcfa9156f59504ab36615dfc4856a)

__Check what RSAT tools are installed__
```powershell
Get-WindowsCapability -Name RSAT* -Online | Select-Object -Property Name, State
```

__Install RSAT Tool__
```powershell-session
Get-WindowsCapability -Name RSAT* -Online | Add-WindowsCapability –Online
```

Once installed, all of the tools will be available under `Administrative Tools` in the `Control Panel`.

__Runas User__
```cmd-session
runas /netonly /user:htb.local\jackie.may powershell
```

__Microsoft Management Console Runas Domain User__
- We can open the `MMC Console` from a non-domain joined computer
```cmd-session
runas /netonly /user:Domain_Name\Domain_USER mmc
```

---
#### LDAP Search Filters
[Active Directory: LDAP Syntax Filters](https://social.technet.microsoft.com/wiki/contents/articles/5392.active-directory-ldap-syntax-filters.aspx)

##### Operators
```powershell
(|(& (..C1..) (..C2..))(& (..C3..) (..C4..)))
``` 
translates to 
```powershell
(C1 AND C2) OR (C3 AND C4)
```

##### Search Criteria
![[Pasted image 20230304101726.png]]

>- [User attirbutes List](http://www.kouti.com/tables/userattributes.htm)  
>- [Base Attributes List](http://www.kouti.com/tables/baseattributes.htm) 

##### Object Identifiers (OIDs)

![[Pasted image 20230304101956.png]]

##### SearchBase & SearchScope
![[Pasted image 20230304105701.png]]

---
### AD Search Filters
##### Operators
![[Pasted image 20230304032625.png | 250]]

```powershell
Get-ADUser -Filter "name -eq 'sally jones'"
Get-ADUser -Filter {name -eq 'sally jones'}
Get-ADUser -Filter'name -eq "sally jones"'
Get-ADUser -filter {name -like "joe*"}
```

![[Pasted image 20230304033044.png]]

---
### DS Tools
```powershell
dsquery user "OU=Employees,DC=inlanefreight,DC=local" -name * -scope subtree -limit 0 | dsget user -samid -
pwdneverexpires | findstr /V no
```

---
### Windows Management Instrumentation (WMI)
```powershell
Get-WmiObject -Class win32_group -Filter "Domain='INLANEFREIGHT'" | Select Caption,Name
```
---
### AD Service Interfaces (ADSI)
Set of COM interfaces that can query Active Directory.
```powershell
([adsisearcher]"(&(objectClass=Computer))").FindAll() | select Path
```

---
### gpresult - GPO enumeration
__GPO of User__
```cmd
gpresult /r /user:harry.jones
```
__GPO of Computer__
```cmd
gpresult /r /S WS01
```
