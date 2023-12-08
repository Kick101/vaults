```powershell
Import-Module ActiveDirectory
```

#### Users
Retrieve Active Directory User
`Get-ADUser -Identity JohnSmith`
Retrieve All Properties Associated with User
Get-ADUser -Identity JohnSmith -Properties *
Retrieve Selected Properties for User
Get-ADUser -Identity JohnSmith -Properties * | Select-Object -Property sAMAccountName, Name,
Mail
New AD User
New-ADUser -Name "MarySmith" -GivenName "Mary" -Surname "Smith" -DisplayName "MarySmith" -Path
"CN=Users,DC=Domain,DC=Local"