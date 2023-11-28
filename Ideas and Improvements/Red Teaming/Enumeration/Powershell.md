#### Powerview
`Get-NetDomain` : Current Domain Information
`Get-NetDomain -Domain marvel.local`
`Get-DomainSID` : Current Domain SID

#### ActiveDirectory Module
- `Get-ADDomain` : Current Domain information
- `Get-ADDomain -Identity marvel.local`
- `((Get-ADDomain).DomainSID.Value)` : Current Domain SID
- 
