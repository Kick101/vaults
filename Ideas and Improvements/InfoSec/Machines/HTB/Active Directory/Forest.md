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

__LDAP__
- Anonymous bind successful

```bash
windapsearch --dc-ip $IP -u "" --functionality
```


