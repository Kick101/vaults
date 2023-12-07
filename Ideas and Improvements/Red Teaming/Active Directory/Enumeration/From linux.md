### Enumerating Users
__rpcclient__
```bash
enumdomusers
```

__CrackMapExec__
```bash
crackmapexec smb $IP --users
```
```bash
 sudo crackmapexec smb $IP -u htb-student -p Academy_student_AD! --users
```
__LDAP__
```bash
ldapsearch -H $IP -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```

```bash
windapsearch.py --dc-ip $IP -u "" -U
```

__Kerbrute__
- This method does not generate Windows event ID [4625: An account failed to log on](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4625), or a logon failure which is often monitored for.
- This generates event ID [4768: A Kerberos authentication ticket (TGT) was requested](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4768). 
- This will only be triggered if [Kerberos event logging](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/enable-kerberos-event-logging) is enabled via Group Policy.

```bash
kerbrute userenum -d inlanefreight.local --dc-ip $IP /opt/jsmith.txt 
```

