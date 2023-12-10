### Offensive Powershell
#### Detection
- System-wide transcription
- Script Block logging
- AntiMalware Scan Interface (AMSI)
- Constrained Language Mode (CLM) - Integrated w/ Applocker & WDAC (Device Guard)

#### Bypass
__Invisi-Shell__
- Hooks .NET assemblies (System.Management.Automation.dll & Syste) to bypass logging




---
### Offensive C\#
__DefenderCheck.exe__
```powershell
DefenderCheck.exe <binaty>
```
- Replace strings names


__Out-CompressedDll.ps1 (SafetyKatz)__
```powershell
out-compressedDll <binay> out.txt
```
- Replace _compressedMimikatzString_ variable in "Constants.cs" of SafetyzKatz fe
- Replace byte size in "Program.cs" file on line 111 & 116

__BetterSafetyKatz__
- Download "mimikatz_trunk.zip"
- Convert into base64
```powershell
$f = "mimikatz_trunk.zip"
[convert]::ToBase64String([IO.File]::ReadAllBytes($f)) | clip
```
- Modify "program.cs":
	- Add a variable to store above base64 value
	- Comment the code that downloads & accepts mimikatz file
	- Add this after zipstream, `zipStream = Convert.FromBase64String(base64value)`

__ConfuserEx (Rubeus.exe)__
- 
