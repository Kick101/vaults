### Powerview
> Disable execution policy `powershell -ep bypass`
> `.\PowerView.ps1`

##### Domain
- `Get-Domain` : Current Domain Information
- `Get-Domain -Domain marvel.local`
- `Get-DomainSID` : Current Domain SID
- `Get-DomainController` : DC
##### Users
- `Get-DomainUser` : All User
- `Get-DomainUser -Domain marvel.local` : Users of marvel.local domain
- `Get-DomainUser -Identity john` : Info about john

- Check if a given user has local admin access or not
```powershell
foreach ($line in $computers) {Get-NetLocalGroupMember -ComputerName $line | ? {$_.SID -eq $sid}}
```
- Reversible encryption Password set
```powershell
Get-DomainUser | ?{ $_.useraccountcontrol -like "*encr*"} | select name
```
##### Groups & members
- `Get-DomainGroup -Properties Name` : All group names
- `Get-DomainGroup *admin*` : Get all groups names w/ admin in 'em
- `(Get-DomainGroup "Help Desk").memberof` : Retrieve the groups that the "Help Desk" group is a member of
- `Get-DomainGroupMemeber "Domain Admins"` : members of "Domain admins" group
- `Get-DomainGroup -UserName john` : groups of user "john"
- `Get-DomainManagedSecurityGroup` : Get managed security groups
- `Get-NetLocalGroup -ComputerName thanos-dc | select GroupName` : Local groups of current user on local machine
- `Add-DomainGroupMember -Identity 'Domain Admins' -Members testda` : Add group member to "Domain Admins"
- What users can PS-Remote|Win-RM on domain computers
```powershell
 Get-ADComputer -Filter * | %{ Get-NetLocalGroupMember -ComputerName $_.name -GroupName "Remote management users"}
```

##### User sessions
- `Get-NetLoggedon -ComputerName dcorp-adminsrv` : Actively logged users on a computer (local admin rights on target required)
- `Get-LoggedonLocal -ComputerName dcorp-adminsrv` : Get locally logged users on a computer (needs remote registry on the target - started by-default on server OS)
- `Get-LastLoggedOn -ComputerName dcorp-adminsrv` : Get the last logged user on a computer (needs administrative rights & remote registry on the target)

##### Shares
- `Invoke-ShareFinder -Verbose` : Find shares on hosts in current domain
- `Invoke-FileFinder -Verbose` : ind sensitive files on computers in the domain
- `Get-NetFileServer` : Get all fileservers of the domain

##### Computers
- `(Get-DomainComputer).name` : all computers names of Domain
- `Find-DomainUserLocation` : Find domain machines that users are logged on

##### Forests
- `Get-Forest` : Root of the current forest
- `Get-Forest -Forest dc.local`
- `Get-ForestDomain` : Domains of current forest
- `Get-ForestDomain -Forest dc.local`
##### Trusts
- `Get-DomainTrust` : Trust relationships of current domain
- `Get-DomainTrust -Domain dc.local`
- `Get-DomainTrustMapping` : All trusts for our current domain

##### ACL
>`SecurityIdentifier` property tells us _who_ has the given right over an object
- `$sid = ConvertTo-NameToSid joe.evans` : Get SID
- Get ACL of Joe.evans on all objects
```powershell
Get-DomainObjectAcl -Domain inlanefreight.local -ResolveGUIDs -Identity * | ?{ $_.SecurityIdentifier -eq $sid}
``` 
- `Get-PathAcl "\\SQL01\DB_backups"` : ACL of File shares
- DCSync access users
```powershell
$dcsync = Get-ObjectACL "DC=inlanefreight,DC=local" -ResolveGUIDs | ? { ($_.ActiveDirectoryRights -match 'GenericAll') -or ($_.ObjectAceType -match 'Replication-Get')} | Select-Object -ExpandProperty SecurityIdentifier | Select -ExpandProperty value
```
`Convert-SidToName $dcsync`
- Rest Password:
```powershell
Set-DomainUserPassword -Identity testda -AccountPassword
(ConvertTo-SecureString "Password@123" -AsPlainText -Force) -Verbose
```


##### GPO
- `Get-DomainGPO | select displayname` : GPO names list
- `Get-DomainGPO -ComputerName WS01 | select displayname` : GPOs of WS01
- Check whether the "Domain Users" group has any permissions on any GPOs
```powershell
Get-DomainGPO | Get-ObjectAcl | ? {$_.SecurityIdentifier -eq 'S-1-5-21-2974783224-3764228556-2640795941-513'}
```
- `Get-GPO -Guid <grp-di>` : Get GPO details from GPO guid
- 


---
### ActiveDirectory Module
> Generally, _admin_ privileges required and _RSAT Tools_ installed
> Disable execution policy `powershell -ep bypass`
> `.\ADModule.ps1`

##### Domain
- `Get-ADDomain` : Current Domain information
- `Get-ADDomain -Identity marvel.local`
- `((Get-ADDomain).DomainSID.Value)` : Current Domain SID
- `Get-ADDomainController` : DC
##### Users
- `Get-ADUser -Filter * -Properties *` : All users and their properties  
- `Get-ADUser -Identity john` : Info about john
##### Groups & members
- `Get-ADGroup -Filter * -Properties *`
- `Get-ADGroup -Filter {Name -like "*admin*"} | select name` : Admin groups
- `Get-ADGroupMemeber -Identity "Domain Admins" -Recursive` : Members of Domain admins, _recursive_ gets members of any group that is member of the domain admins group
- `Get-ADPrincipalGroupMembership -Identity john` : groups of john
- `Add-ADGroupMember -Identity 'Domain Admins' -Members testda` : Add group member to "Domain Admins"
##### Computers
- `Get-ADComputer -Filter * -Properties *` : all computers
- `Get-ADComputer -filter * -Properties * | select name,description` : Description of computer accounts


##### Forests
- `Get-ADForest`
- `Get-ADForest -Identity dc.local`
- `(Get-ADForest).Domains` : Domains of current forest

##### Trusts
- `Get-ADTrust -Filter *` : Trust relationships of current domain
- `Get-ADTrust -Identity dc.local`

##### ACL
- ACL for a single domain user
```powershell
(Get-ACL "AD:$((Get-ADUser john.doe).distinguishedname)").access  | ? {$_.IdentityReference -eq "INLANEFREIGHT\cliff.moore"}
```
- __ACL of Domain users__
	- `Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt` : Save all users to a file
	- ACE of john on all other users
```powershell
foreach($line in [System.IO.File]::ReadLines("ad_users.txt")) {get-acl  "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match 'INLANEFREIGHT\\john'}}
```
- Reset password
```powershell
Set-ADAccountPassword -Identity testda -NewPassword
(ConvertTo-SecureString "Password@123" -AsPlainText -Force) -Verbose
```


##### Using LDAP Filter
__Workstations in a network__
```powershell
Get-ADObject -LDAPFilter '(objectCategory=computer)'
```

__Domain Controllers__
```powershell
Get-ADObject -LDAPFilter '(&(objectCategory=Computer)(userAccountControl:1.2.840.113556.1.4.803:=8192))'
```

__User Related__
```powershell
Get-ADObject -LDAPFilter '(&(objectCategory=person)(objectClass=user))'
```


__All AD groups__
```powershell
Get-ADObject -LDAPFilter '(objectClass=group)' | select Name
```

__Disabled Accounts__
```powershell
Get-ADObject -LDAPFilter '(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=2))' | select samaccountname,useraccountcontrol
```

__Find all groups that user belongs to__
```powershell
Get-ADGroup -LDAPFilter '(member:1.2.840.113556.1.4.1941:=CN=Harry Jones,OU=Network Ops,OU=IT,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL)' | select Name
```
__Get all description fields__
```powershell
Get-ADUser -Properties * -LDAPFilter '(&(objectCategory=user)(description=*))' | select samaccountname,description
```

__Find Trusted Users__
```powershell
Get-ADUser -Properties * -LDAPFilter '(userAccountControl:1.2.840.113556.1.4.803:=524288)' | select Name,memberof, servicePrincipalName,TrustedForDelegation | fl
```
__Find Trusted Computers__
```powershell
Get-ADComputer -Properties * -LDAPFilter '(userAccountControl:1.2.840.113556.1.4.803:=524288)' | select DistinguishedName,servicePrincipalName,TrustedForDelegation | fl
```
__Admin accts for which password not required__
```powershell
Get-AdUser -LDAPFilter '(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))(adminCount=1)' -Properties * | select name,memberof | fl
```

__Under what group current group is nested into__
```powershell
Get-ADObject -LDAPFilter "(name=IT Support)" -Properties memberOf | Select-Object -ExpandProperty memberOf
```

__All Groups of a user__
```powershell
Get-ADGroup -LDAPFilter '(member:1.2.840.113556.1.4.1941:=CN=Harry Jones,OU=Network Ops,OU=IT,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL)' |select Name
```

__Find SQL service accts__
```powershell
Get-ADComputer -Filter "DNSHostName -like 'SQL*'"
```
__Find Computer name using LIKE filter__
```powershell
Get-ADComputer -Filter {Name -like "RD*"}
```
__Find Administrative Groups__
```powershell
Get-ADGroup -Filter "adminCount -eq 1" | select Name
```

__Get all OUs in a DC__
```powershell
Get-ADOrganizationalUnit -Filter * -Server <DC_Name> | Select-Object Name, DistinguishedName, Description, WhenCreated
```

__Who is a part of this group through nested group membership__
```powershell
Get-ADGroupMember "Server Technicians" -Recursive | Where-Object { $_.objectClass -eq "user" } | Select-Object Name, SamAccountName
```

__Get all Protected Users__
```powershell
Get-ADGroupMember -Identity "Protected Users" -Recursive | Get-ADUser | Select-Object Name, SamAccountName
```

__Find _ASREPRoasteable_ Admin accts__
```powershell
Get-ADUser -Filter {adminCount -eq '1' -and DoesNotRequirePreAuth -eq 'True'}
```

__Find admin accts subject to _Kerberoasting___
```powershell
Get-ADUser -Filter "adminCount -eq '1'" -Properties * | where servicePrincipalName -ne $null | select SamAccountName,MemberOf,ServicePrincipalName | fl
```

__All Groups of User (Recursive)__
```powershell
Get-ADGroup -Filter 'member -RecursiveMatch "CN=Harry Jones,OU=Network Ops,OU=IT,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL"' | select name
```

__All Ous__
```powershell
Get-ADOrganizationalUnit -Filter * -Properties name, distinguishedName | Select-Object name, distinguishedName
```

__Get all objects in a OU__
```powershell
Get-ADUser -SearchBase "OU=Employees,DC=INLANEFREIGHT,DC=LOCAL" -SearchScope OneLevel -Filter *
```

__Get all objects from child OUs & OU__
```powershell
Get-ADUser -SearchBase "OU=Employees,DC=INLANEFREIGHT,DC=LOCAL" -SearchScope Subtree -Filter *
```

__Get All users in a OU__
```powershell
Get-ADUser -Filter * -SearchBase "OU=IT,OU=Employees,DC=INLANEFREIGHT,DC=LOCAL"
```

__Pulling Date User added to Group__
- _Get-ADGroupMemberDate_ can be downloaded [here](https://gallery.technet.microsoft.com/scriptcenter/Find-the-time-a-user-was-a0bfc0cf/view/Discussions).

```powershell
Get-ADGroupMemberDate -Group "Help Desk" -DomainController DC01.INLANEFREIGHT.LOCAL
```

```powershell
Get-ADGroupMemberDate -Group "Help Desk" -DomainController DC01.INLANEFREIGHT.LOCAL | ? { ($_.Username -match 'harry.jones') -And ($_.State -NotMatch 'ABSENT') }
```

__Check if a given user has local admin access or not__
- Another function to do this quickly?
```powershell
$computers = Get-DomainComputer -Properties dnshostname | select -ExpandProperty dnshostname
```

---
### Sharpview
##### Groups
- `Get-NetLocalGroupMember -ComputerName WS01` : Local groups
- 

---
### Powershell 
##### Find Local Admin Access
Find all machines on domain where current user has local admin access
This leaves _4624, 4634, 4672_
- `Invoke-EnumerateLocalAdmin -verbose` 
- `Find-LocalAdminAccess -verbose` 

##### List Sessions
- `Get-NetSession -ComputerName ops-dc` : Find boxes where a Domain Admin is logged in and current user has access
- `Invoke-UserHunter -CheckAccess` : 

##### Access Control Lists
- `Get-ObjectAcl -SAMAccountName john -ResolveGUIDs` : Access control lists of john. _ActiveDirectoryRights_
- `Get-ObjectAcl -ADSprefix 'CN=Administrator.CN=Users' -Verbose` : ACLs associated w/ specified prefix
- `(Get-Acl 'AD:\CN=john,CN=Users,DC=offensiveps,DC=powershell,DC=local').Acess` : ActiveDirectory Module
- `Invoke-ACLScanner -ResolveGUIDS` : Interesting ACLs

##### Misc
__Installed Software__
```powershell
get-ciminstance win32_product | fl
```

__Filter out microsoft s/w__
```powershell
get-ciminstance win32_product -Filter "NOT Vendor like '%Microsoft%'" | fl
```

##### Enumerating ACLs with Built-In Cmdlets

__Search out objects with modification rights over non-built-in objects__
```powershell
Find-InterestingDomainAcl -Domain inlanefreight.local -ResolveGUIDs
```

__ACLs set on file shares__
- List out shares
```powershell
Get-NetShare -ComputerName SQL01
```
- Get ACLs
```powershell
 Get-PathAcl "\\SQL01\DB_backups"
```


---
### ADSI
__Users__
```powershell
$Searcher = New-Object DirectoryServices.DirectorySearcher;
$Searcher.Filter = "(&(objectclass=user))";
$Searcher.SearchRoot = '';
$Searcher.FindAll();
```

__Computers__
```powershell
$Searcher.Filter = "(&(objectclass=user))";
$Searcher.SearchRoot = '';
$Searcher.FindAll();
```

__Trusts__
```powershell
([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).GetAllTrustRelationships()
```

__Users w/ SPNs__
```powershell
$Searcher.Filter = "
(&(!(samaccountname=krbtgt))(objectclass=user)(objectcategory=user)(servicePrincipalName=*))";
$Searcher.SearchRoot = '';
$Searcher.FindAll();
```

---
### PowerUpSQL
__SQL Servers__
```powershell
Get-SQLInstanceDomain
```

__DB links__
```powershell
Get-SQLServerLink -Instance devsrv.garrison.castle.local
```

```powershell
Get-SQLServerLinkCrawl -Instance devsrv.garrison.castle.local -Verbose
```



---
### Security Controls
__Checking the Status of Defender with Get-MpComputerStatus__
- Check `RealTimeProtectionEnabled` value
```powershell
Get-MpComputerStatus
```

__Using Get-AppLockerPolicy cmdlet__
- Applocker is application whitelisting solution
- Organizations block the `PowerShell.exe`, but forget about the other [PowerShell executable locations](https://www.powershelladmin.com/wiki/PowerShell_Executables_File_System_Locations) such as `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe` or `PowerShell_ISE.exe`.

```powershell
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

__PowerShell Constrained Language Mode__
- Locks down features needed to use PowerShell effectively, such as blocking COM objects, only allowing approved .NET types, XAML-based workflows, PowerShell classes
```powershell
$ExecutionContext.SessionState.LanguageMode
```

__Local Administrator Password Solution__
- LAPS is used to _randomize and rotate local administrator passwords_ on Windows hosts and prevent lateral movement.
- We can enumerate what domain users can read the LAPS password and what machines do not have LAPS installed.
- An account that has joined a computer to a domain receives `All Extended Rights` over that host, and this right gives the account the ability to read passwords


```powershell
Find-LAPSDelegatedGroups
```

Checks the rights on each computer with LAPS enabled for any groups with read access and users with "All Extended Rights."
```powershell
Find-AdmPwdExtendedRights
```

Search for computers that have LAPS enabled when passwords expire, and even the randomized passwords in cleartext if our user has access.
```powershell
Get-LAPSComputers
```


