### Powerview
> Disable execution policy `powershell -ep bypass`
> `.\PowerView.ps1`

##### Domain
- `Get-NetDomain` : Current Domain Information
- `Get-NetDomain -Domain marvel.local`
- `Get-DomainSID` : Current Domain SID
- `Get-NetDomainController` : DC
##### Users
- `Get-NetUser` : All User
- `Get-NetUser -Domain marvel.local` : Users of marvel.local domain
- `Get-NetUser -UserName john` : Info about john
##### Groups & members
- `Get-NetGroup` or `Get-DomainGroup -Properties Name` : All group names
- `Get-NetGroup *admin*` : Get all groups names w/ admin in 'em
- `Get-NetGroupMemeber -GroupName "Domain Admins"`or `Get-DomainGroupMember -Identity Help-Desk`: members of Domain admins
- `Get-NetGroup -UserName john` : groups of john
- `Get-DomainManagedSecurityGroup` : Get managed security groups
##### Computers
- `Get-NetComputer` : all computers of Domain
- `Get-NetComputer -FullData` : Full data of computers, queries are made to DC
##### Forests
- `Get-NetForest` : Root of the current forest
- `Get-NetForest -Forest dc.local`
- `Get-NetForestDomain` : Domains of current forest
- `Get-NetForestDomain -Forest dc.local`
##### Trusts
- `Get-NetDomainTrust` : Trust relationships of current domain
- `Get-NetDomainTrust -Domain dc.local`

##### Access Controls
- `ConvertTo-SID joe.evans` : Get SID
- `Get-DomainObjectAcl -Identity 'Security Operations' | ?{ $_.SecurityIdentifier -eq $sid}` : Get ACL of Joe evans, `$sid` is 
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
##### Computers
- `Get-ADComputer -Filter * -Properties *` : all computers

##### Forests
- `Get-ADForest`
- `Get-ADForest -Identity dc.local`
- `(Get-ADForest).Domains` : Domains of current forest

##### Trusts
- `Get-ADTrust -Filter *` : Trust relationships of current domain
- `Get-ADTrust -Identity dc.local`

---
### Sharpview

---
### Powershell 
##### Local Admin Access
Find all machines on domain where current user has local admin access
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




