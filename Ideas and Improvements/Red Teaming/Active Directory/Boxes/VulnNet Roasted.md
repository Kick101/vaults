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
1105: VULNNET-RST\a-whitehat (SidTypeUser)
1109: VULNNET-RST\t-skid (SidTypeUser)
1110: VULNNET-RST\j-goldenhand (SidTypeUser)
1111: VULNNET-RST\j-leet (SidTypeUser)
```

