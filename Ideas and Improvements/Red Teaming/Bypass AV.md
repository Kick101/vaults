### String Manipulation
__DefenderCheck.exe__
```powershell
DefenderCheck.exe <binaty>
```

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
	- Comment 