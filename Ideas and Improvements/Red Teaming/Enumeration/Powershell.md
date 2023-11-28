#### Powerview
> Disable execution policy `powershell -ep bypass`
> .`\PowerView.ps1`

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
- `Get-NetGroup ` : All group names
- `Get-NetGroup *admin*` : Get all groups names w/ admin in 'em
- `Get-NetGroupMemeber -GroupName "Domain Admins"` : members of Domain admins
- `Get-NetGroup -UserName john` : groups of john
##### Computers
- `Get-NetComputer` : all computers of Domain
- `Get-NetComputer -FullData` : Full data of computers, queries are made to DC



---
#### ActiveDirectory Module
> Generally, _admin_ privileges required and _Rset Tools_ installed
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

---
#### Powershell 
##### Local Admin Access
Find all machines on domain where current user has local admin access
- `Invoke-EnumerateLocalAdmin -verbose` 
- `Find-LocalAdminAccess -verbose` 

##### List Sessions
- `Get-NetSession -ComputerName ops-dc` : Find boxes where a Domain Admin is logged in and current user has access
- `Invoke-UserHunter -CheckAccess` : 

##### Access Control Lists
- `Get-ObjectAcl -SAMAccountName john -ResolveGUIDs`
- 

