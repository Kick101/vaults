```powershell
Import-Module ActiveDirectory
```

#### Users
__Retrieve Active Directory User__
```powershell
Get-ADUser -Identity JohnSmith
```
__Retrieve All Properties Associated with User__
```powershell
Get-ADUser -Identity JohnSmith -Properties *
```
__Retrieve Selected Properties for User__
```powershell
Get-ADUser -Identity JohnSmith -Properties * | Select-Object -Property sAMAccountName, Name,Mail
```
__New AD User__
```powershell
New-ADUser -Name "MarySmith" -GivenName "Mary" -Surname "Smith" -DisplayName "MarySmith" -Path "CN=Users,DC=Domain,DC=Local"
```
#### Groups
__Retrieve Active Directory Group__
```powershell
Get-ADGroup -Identity "My-First-Group"
```
__Retrieve All Properties Associated with Group__
```powershell
Get-ADGroup -Identity "My-First-Group" -Properties *
```
__Retrieve All Members of a Group__
```powershell
Get-ADGroupMember -Identity "My-First-Group" | Select-Object -Property sAMAccountName
```
```powershell
Get-ADgroup "MY-First-Group" -Properties Members | Select -ExpandProperty Members
```
__Add AD User to an AD Group__
```powershell
Add-ADGroupMember -Identity "My-First-Group" -Members "JohnSmith"
```
__New AD Group__
```powershell
New-ADGroup -GroupScope Universal -Name "My-Second-Group"
```
#### Computers
__Retrieve AD Computer__
```powershell
Get-ADComputer -Identity "JohnLaptop"
```
__Retrieve All Properties Associated with Computer__
```powershell
Get-ADComputer -Identity "JohnLaptop" -Properties *
```
__Retrieve Select Properties of Computer__
```powershell
Get-ADComputer -Identity "JohnLaptop" -Properties * | Select-Object -Property Name, Enabled
```
#### Objects
__Retrieve an Active Directory Object__
```powershell
Get-ADObject -Identity "ObjectGUID07898"
```
__Move an Active Directory Object__
```powershell
Move-ADObject -Identity "CN=JohnSmith,OU=Users,DC=Domain,DC=Local" -TargetPath "OU=SuperUser,DC=Domain,DC=Local"
```
__Modify an Active Directory Object__
```powershell
Set-ADObject -Identity "CN=My-First-Group,OU=Groups,DC=Domain,DC=local" -Description "This is My First Object Modification"
```


