#### Enumeration
##### Nmap
```bash
rustscan -a $IP -- -A -T4 -oN nmap.log
```

__SMB__
- Anonymous logic successful but no shares found

```bash
smbclient -L //$IP/
```
- Account Lockout Threshold: None
```bash
crackmapexec smb $IP --pass-pol
```

__LDAP__
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


