**Sysmon's primary components include:**
- A Windows service for monitoring system activity.
- A device driver that assists in capturing the system activity data.
- An event log to display captured activity data.

### Install sysmon

```powershell
sysmon.exe -i -accepteula -h md5,sha256,imphash -l -n
```

---
## 🔍 Sysmon Event IDs: 1–25

| Event ID | Event Type                       | Description                                                                        | Common Use Case                                                |
| -------- | -------------------------------- | ---------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| **1**    | Process Create                   | Logs every new process creation. Includes command-line, hashes, parent/child info. | Track malware execution, lateral movement, suspicious scripts. |
| **2**    | File Creation Time Changed       | Logs changes in file creation timestamps (anti-forensics).                         | Detect timestomping (e.g., to evade IR).                       |
| **3**    | Network Connection               | Logs outbound TCP connections (remote IP, port, process).                          | Detect beaconing, C2 communication, data exfil.                |
| **4**    | Sysmon Service State Change      | Logs when Sysmon starts or stops.                                                  | Detect service tampering.                                      |
| **5**    | Process Terminate                | Logs when a process exits.                                                         | Useful in combination with EID 1.                              |
| **6**    | Driver Loaded                    | Logs drivers (kernel modules) loaded into memory.                                  | Rootkit detection.                                             |
| **7**    | Image Loaded                     | Logs DLLs or binaries loaded into a process. Can be filtered for known libraries.  | DLL injection, LOLBins, reflective loading.                    |
| **8**    | CreateRemoteThread               | Logs remote thread injection into another process.                                 | Key indicator of process injection.                            |
| **9**    | RawAccessRead                    | Logs raw disk access (bypasses filesystem).                                        | Used by disk dumpers, forensic tools.                          |
| **10**   | ProcessAccess                    | Logs access to another process (e.g., reading memory, tokens).                     | Mimikatz, credential stealing.                                 |
| **11**   | FileCreate                       | Logs file creation (not writes).                                                   | Dropped binaries, persistence artifacts.                       |
| **12**   | Registry Object Added/Created    | Registry key/values added.                                                         | Persistence detection (e.g., Run keys).                        |
| **13**   | Registry Object Modified         | Registry value data modified.                                                      | Tampering with configs, services.                              |
| **14**   | Registry Object Deleted          | Registry key or value deleted.                                                     | Anti-forensic behavior.                                        |
| **15**   | FileCreateStreamHash             | Logs alternate data streams created. Includes hash.                                | ADS usage — often used by malware.                             |
| **16**   | Sysmon Configuration Change      | Logs changes to Sysmon’s configuration.                                            | Detect tampering with logging config.                          |
| **17**   | Pipe Created                     | Named pipes created.                                                               | Used for IPC or malware C2 channels.                           |
| **18**   | Pipe Connected                   | Connection to named pipe.                                                          | Further IPC activity.                                          |
| **19**   | WmiEventFilter activity detected | WMI event filter creation.                                                         | WMI persistence or lateral movement.                           |
| **20**   | WmiEventConsumer activity        | WMI event consumer created.                                                        | Often used in attacks for persistence.                         |
| **21**   | WmiEventConsumerToFilter         | Link between filter and consumer.                                                  | Ties WMI filter-consumer pairs together.                       |
| **22**   | DNS Query                        | Logs DNS queries (domain names, process making the query).                         | Detect domain lookups for C2 or phishing.                      |
| **23**   | FileDelete                       | Logs file deletions. (Only in versions 13.0+ with FileDelete rule.)                | Tracks cleanup attempts by malware.                            |
| **24**   | Clipboard Change                 | Logs clipboard changes (requires enabling in config).                              | Data exfiltration via clipboard.                               |
| **25**   | Process Tampering                | Detects process hollowing, PEB spoofing, etc.                                      | Key for malware evasion detection (e.g., ransomware, APTs).    |

---
### Detection  #1: DLL Hijacking

- To detect a DLL hijack, we need to focus on `Event Type 7` , which corresponds to module load events. To achieve this, we need to modify the `sysmonconfig-export.xml`
- Let's attempt the hijack using "`calc.exe`" and "`WININET.dll`" as an example. 
- To simplify the process, we can utilize *Stephen Fewer's "hello world" reflective DLL*. It should be noted that DLL hijacking does not require reflective DLLs. By following the required steps, which involve renaming reflective_dll.x64.dll to WININET.dll, moving calc.exe from `C:\Windows\System32` along with `WININET.dll` to a writable directory (such as the  Desktop folder), and executing `calc.exe` , we achieve success. Instead of the Calculator application, a MessageBox is displayed.

In the case of detecting DLL hijacks, we change the "include" to "exclude" to ensure that
nothing is excluded, allowing us to capture the necessary data.

![[Pasted image 20250519200744.png]]
```powershell
sysmon.exe -c sysmonconfig-export.xml
```

#### IOCs
1. "`calc.exe`", originally located in System32, should not be found in a writable directory. Therefore, a copy of "calc.exe" in a writable directory serves as an IOC, as it should always reside in `System32` or potentially `Syswow64`.
2. "`WININET.dll`", originally located in System32, should not be loaded outside of System32 by calc.exe. If instances of "WININET.dll" loading occur outside of System32 with "calc.exe" as the parent process, it indicates a DLL hijack within calc.exe. While caution is necessary when alerting on all instances of "WININET.dll" loading outside of System32 (as ***some applications may package specific DLL versions for stability***), in the case of "calc.exe", we can confidently assert a hijack due to the DLL's unchanging name, which attackers cannot modify to evade detection.
3. The original "WININET.dll" is Microsoft-signed, while our injected DLL remains unsigned.

---
### Detection  #2: Unmanaged PowerShell/C-Sharp Injection

**Brief about of C# and its runtime environment**
- C# is a "managed" language, meaning it requires a runtime environment to execute code.
- The **Common Language Runtime (CLR)** serves as this backend runtime for C#.
- Managed code is **not compiled directly to assembly**; instead, it's compiled into **bytecode** (Intermediate Language or IL).
- The CLR then **processes and executes** this bytecode at runtime.
- As a result, a **managed process relies on the CLR** to run C# code.

By using `Process Hacker`, we can observe a range of processes within our environment. Sorting the processes by name, we can identify interesting color-coded distinctions. Notably, "`powershell.exe`", a managed process, is highlighted in green compared to other processes. Hovering over `powershell.exe` reveals the label "**Process is managed (.NET),**" confirming its managed status.

![[Pasted image 20250519202055.png|500]]

- Examining the module loads for `powershell.exe` , by right-clicking on powershell.exe, clicking "Properties", and navigating to "Modules", we can find relevant information.

![[Pasted image 20250519202132.png]]

- The presence of "Microsoft .NET Runtime...", `clr.dll` , and `clrjit.dll` should attract our attention. ***These 2 DLLs are used when C# code is ran as part of the runtime to execute the bytecode***. If we observe these DLLs loaded in processes that typically do not require them, it suggests a potential ***execute-assembly or unmanaged PowerShell injection attack***.

#### Attack

- To showcase unmanaged PowerShell injection, we can inject an unmanaged PowerShell-like DLL into a random process, such as `spoolsv.exe` . We can do that by utilizing the `PSInject project` in the following manner.

```powershell
powershell -ep bypass
Import-Module .\Invoke-PSInject.ps1
Invoke-PSInject -ProcId [Process ID of spoolsv.exe] 
-PoshCode "V3JpdGUtSG9zdCAiSGVsbG8sIEd1cnU5OSEi"
```
- After the injection, we observe that "`spoolsv.exe`" transitions from an unmanaged to a managed state.

![[Pasted image 20250519202926.png|500]]

- Additionally, by referring to both the related "Modules" tab of Process Hacker and Sysmon `Event ID 7` , we can examine the DLL load information to validate the presence of the aforementioned DLLs.

![[Pasted image 20250519203039.png | 500]]

![[Pasted image 20250519203059.png|500]]

---
### Detection #3: Credential Dumping

By checking Sysmon `event ID 10` , which represents "ProcessAccess" events, we can identify any suspicious attempts to access `LSASS`.

![[Pasted image 20250519203420.png]]

![[Pasted image 20250519203513.png]]

For instance, if we observe a random file ("`AgentEXE`" in this case) from a random folder
("Downloads" in this case) attempting to access LSASS, it indicates unusual behavior.
Additionally, the `SourceUser` being different from the `TargetUser` (e.g., "waldo" as the
SourceUser and "`SYSTEM`" as the TargetUser ) further emphasizes the abnormality. It's
also worth noting that as part of the `mimikatz`-based credential dumping process, the user
must request `SeDebugPrivileges`. As the name suggests, it's primarily used for debugging.
This can be another Indicator of Compromise (IOC).

>Please note that some legitimate processes may access LSASS, such as authentication-related processes or security tools like AV or EDR.





---
### References
https://github.com/SwiftOnSecurity/sysmon-config
https://github.com/olafhartong/sysmon-modular
https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon