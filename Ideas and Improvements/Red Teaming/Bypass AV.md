### Offensive Powershell
#### Detection
- System-wide transcription
- Script Block logging
- AntiMalware Scan Interface (AMSI)
- Constrained Language Mode (CLM) - Integrated w/ Applocker & WDAC (Device Guard)

#### Bypass
- Invisi-Shell
- AMSITrigger
- DefenderCheck

__Invisi-Shell__
- Bypasses AMSI, System-wide transcription & script block logging
- Hooks .NET assemblies (System.Management.Automation.dll & System.Core.dll) to bypass logging
- CLR Profiler API is used perform the hook
- _CLR profiler_ is a dll that receives and send messages to the CLR by using API

Using:
- RunwithPathAsAdmin.bat
- RunwithRegistryNonAdmin.bat (Use this preferably)

__AMSITrigger__
```powershell
AmsiTrigger_x64.exe -i PowerUp.ps1
```
- Reverse the string
```powershell
$string = 'niamoDppA.metsys'
$classrev = ([regx]::Matches($String,'.','RightToLeft') | ForEach {$_.value}) - join ''
```

#### Invoke-Mimikatz
- Remove default comments
- Rename script, variables, functions
- Obfuscate PE Bytes content -> Obfuscate PowerKatz dll using _protectMyTooling_
- Convert powerkatz dll into base64 & reverse it again
- Add sandbox check to waster dynamic analysis resources
- Remove reflective PE warnings
- Use obfuscated commands `Invoke-Mimmi -Command $sfsd`
	- `$sfsd = $a + $b`
	- `$a="s"; $b="Eku"`
- Analysis using DenfenderCheck
- DefenderCheck



---
### Offensive C\#
#### Bypass
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
