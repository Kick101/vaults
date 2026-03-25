### MDE
#### ETW (Event Tracing for Windows)

#### ASR Rules (Attack Surface Reduction)

---
#### Credential Extraction - LSASS Dump
- MiniDumpDotNet - Rewritten `MiniDumpWriteDump` API call

```powershell
.\minidumpnotnet.exe $LSASSPID <miniDumpFile>
```
##### Detection
Below 3 actions are heavily monitored by EDR
- Gaining a handle to the LSASS process
- Creating a minidump w/ MiniDumpWriteDump WinAPI call, implemented in `dbghelp.dll`/`dbgcore.dll`
- Writing the dump file on disk
 
#### Tools Transfer
- SMB Share
- `msedge.exe`

#### Breaking Detection Chains
- Wait for some time (10 min)
- Do non-suspicious activities in b/w sequences

#### ASR Rules
- Can be extracted from target machine
```powershell
WSManWinRM.exe eu-sql6.eu.eurocorp.local "cmd /c notepad.exe C:\Windows\ccm\systemtemp\"
```

#### Protection Detection
- `set username` `set computername`
- 




