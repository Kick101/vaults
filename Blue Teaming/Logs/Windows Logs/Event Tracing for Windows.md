According to Microsoft, Event Tracing For Windows (ETW) is a general-purpose, high-
speed tracing facility provided by the operating system. Using a **buffering and logging**
**mechanism** implemented in the kernel, ETW provides a tracing mechanism for events raised
by both user-mode applications and kernel-mode device drivers.

---
## ETW Architecture & Components

![[Pasted image 20250520103946.png]]

#### Controllers 
The Controllers component, as its name implies, assumes control over all aspects related to ETW operations. It encompasses functionalities such as **initiating and terminating trace sessions**, as well as **enabling or disabling providers** within a particular trace. Trace sessions can establish subscriptions to one or multiple providers, thereby granting the providers the ability to commence logging operations. An example of a widely used controller is the built-in utility "`logman.exe`"

**At the core of ETW's architecture is the publish-subscribe model involves two primary components:**
- Providers
- Consumers

#### Providers 

Providers play a pivotal role in generating events and writing them to the designated ETW sessions. Applications have the ability to register ETW providers, enabling them to generate and transmit numerous events. There are ***four distinct types of providers*** utilized within ETW.
- **MOF Providers :** These providers are based on **Managed Object Format** (MOF) and are capable of generating events according to predefined *MOF schemas*. They offer a flexible approach to event generation and are widely used in various scenarios.
- **WPP Providers :** Standing for "**Windows Software Trace Preprocessor**," WPP providers leverage specialized *macros and annotations* within the application's source code to generate events. This type of provider is often utilized for ***low-level kernel-mode tracing and debugging purposes***.
- **Manifest-based Providers :** Manifest-based providers represent a more contemporary form of providers within ETW. They rely on *XML manifest files* that define the structure and characteristics of events. This approach offers enhanced flexibility and ease of management, allowing for dynamic event generation and customization.
- **TraceLogging Providers :** TraceLogging providers offer a simplified and efficient approach to event generation. They leverage the **TraceLogging API**, introduced in recent Windows versions, which streamlines the process of event generation with minimal code overhead.

#### Consumers 
 
 Consumers subscribe to specific events of interest and receive those events for further processing or analysis. By default, the events are typically directed to an `.ETL` (Event Trace Log) file for handling. However, an alternative consumer scenario involves leveraging the capabilities of the Windows API to process and consume the events.

#### Channels 

To facilitate efficient event collection and consumption, ETW relies on event
channels. Event channels act as *logical containers* for organizing and filtering events
based on their characteristics and importance. ETW supports multiple channels, each
with its own defined purpose and audience. Event consumers can selectively subscribe
to specific channels to receive relevant events for their respective use cases.

#### ETL files

ETW provides specialized support for *writing events to disk* through the use of event trace log files, commonly referred to as "ETL files." These files serve as durable storage for events, enabling offline analysis, long-term archiving, and forensic investigations. ETW allows for seamless rotation and management of ETL files to ensure efficient storage utilization.


>#### Notes
>- ETW supports event providers in both kernel mode and user mode.
> - Some event providers generate a significant volume of events, which can potentially overwhelm the system resources if they are constantly active. As a result, to prevent unnecessary resource consumption, these providers are typically disabled by default and are only enabled when a tracing session specifically requests their activation.
> - In addition to its inherent capabilities, ETW can be extended through custom event providers.
> - Only ETW provider events that have a Channel property applied to them can be consumed by the event log


---
## Interacting With ETW

- `Logman` is a pre-installed utility for managing Event Tracing for Windows (ETW) and Event Tracing Sessions. This tool is invaluable for *creating, initiating, halting, and investigating* *tracing sessions*. 
- This is particularly useful when determining which sessions are set for data collection or when initiating your own data collection. 
- Employing the `-ets` parameter will allow for a direct investigation of the event tracing sessions, providing insights into system-wide tracing sessions. 
- Example, the **Sysmon Event Tracing Sessions** can be found towards the end of the displayed information.

When we examine an Event Tracing Session directly, we uncover specific session details
including the **Name, Max Log Size, Log Location, and the subscribed providers**. 

**For each provider subscribed to the session, we can acquire critical data:**

- **Name / Provider GUID :** This is the exclusive identifier for the provider.
- **Level :** This describes the event level, indicating if it's filtering for *warning, informational, critical*, or all events.
- **Keywords Any :** Keywords create a filter based on the kind of event generated by the provider.

```powershell
logman.exe query "EventLog-System" -ets
```

By using the `logman query providers` command, we can generate a list of all available
providers on the system, including their respective GUIDs.

```powershell
logman.exe query providers
```

You will see multiple results for "`Winlogon`" in the given example
```powershell
logman.exe query providers | findstr "Winlogon"
```


**Specifying a provider :** we gain a deeper understanding of the provider's function. This will inform us about the Keywords we can filter on, the available event levels, and which processes are currently utilizing the provider.
```powershell
logman.exe query providers Microsoft-Windows-Winlogon
```

The `Microsoft-Windows-Winlogon/Diagnostic` and `Microsoft-Windows-Winlogon/Operational` keywords reference the event logs generated from this provider.

#### GUI-based alternatives
1. **Performance Monitor tool**, we can visualize various running trace sessions. A detailed overview of a specific trace can be accessed simply by double-clicking on it. This reveals all pertinent data related to the trace, from the engaged providers and their activated features to the nature of the trace itself. Additionally, these sessions can be modified to suit our needs by incorporating or eliminating providers. Lastly, we can devise new sessions by opting for the "User Defined" category.
2. ETW Provider metadata can also be viewed through the `EtwExplorer` project.


---
## Useful Providers

| **ETW Provider**                                       | **Focus Area**             | **Security Use Cases**                                                  |
| ------------------------------------------------------ | -------------------------- | ----------------------------------------------------------------------- |
| Microsoft-Windows-Kernel-Process                       | Process activity           | Detect process injection, hollowing, malware/APT behavior               |
| Microsoft-Windows-Kernel-File                          | File operations            | Detect unauthorized access, ransomware, file tampering                  |
| Microsoft-Windows-Kernel-Network                       | Network activity           | Identify C2 communication, exfiltration, unauthorized connections       |
| Microsoft-Windows-SMBClient / SMBServer                | SMB traffic                | Monitor lateral movement, unusual file sharing or data exfiltration     |
| Microsoft-Windows-DotNETRuntime                        | .NET runtime execution     | Identify malicious assemblies, .NET exploitation                        |
| OpenSSH                                                | SSH connections            | Detect brute-force attacks, failed logins, unauthorized access attempts |
| Microsoft-Windows-VPN-Client                           | VPN usage                  | Detect suspicious or unauthorized VPN activity                          |
| Microsoft-Windows-PowerShell                           | PowerShell commands        | Catch suspicious scripting activity, misuse, or exploitation            |
| Microsoft-Windows-Kernel-Registry                      | Registry operations        | Spot persistence mechanisms, malware installations, config changes      |
| Microsoft-Windows-CodeIntegrity                        | Code/driver integrity      | Detect loading of unsigned/malicious code or drivers                    |
| Microsoft-Antimalware-Service                          | Antimalware service status | Identify evasion attempts, service disabling, config tampering          |
| WinRM                                                  | Remote management          | Detect lateral movement, remote command execution                       |
| Microsoft-Windows-TerminalServices-LocalSessionManager | Terminal Services sessions | Spot unauthorized/suspicious RDP activity                               |
| Microsoft-Windows-Security-Mitigations                 | Security mitigations       | Monitor bypass attempts of OS-level security features                   |
| Microsoft-Windows-DNS-Client                           | DNS client requests        | Detect DNS tunneling, suspicious DNS requests (potential C2)            |
| Microsoft-Antimalware-Protection                       | Antimalware operations     | Monitor protection status, configuration, and evasion attempts          |

----
## Detection #1: Strange Parent-Child Relationships
Abnormal parent-child relationships among processes can be indicative of malicious activities. In standard Windows environments, certain processes never call or spawn others. For example, it is highly unlikely to see "calc.exe" spawning "cmd.exe" in a normal Windows environment.

By utilizing Process Hacker, we can explore parent-child relationships within Windows.
Sorting the processes by dropdowns in the Processes view reveals a hierarchical representation of the relationships.

### Parent PID Spoofing Attack
Parent PID Spoofing can be executed through the `psgetsystem` project in the following manner.

```powershell
powershell -ep bypass
Import-Module .\psgetsys.ps1
[MyProcess] ::CreateProcessFromParent( [Process ID of spoolsv.exe] ,"C:\Windows\System32\cmd.exe","")
```
![[Pasted image 20250520125427.png ]]

- Due to the parent PID spoofing technique we employed, Sysmon Event 1 incorrectly displays `spoolsv.exe` as the parent of `cmd.exe` . However, it was actually `powershell.exe` that created `cmd.exe` .
- Let's begin by collecting data from the `Microsoft-Windows-Kernel-Process` provider using `SilkETW` (the provider can be identified using logman, `logman.exe query providers | findstr "Process"` ). 
- After that, we can proceed to simulate the attack again to assess whether ETW can provide us with more accurate information regarding the execution of `cmd.exe` .

```powershell
SilkETW.exe -t user -pn Microsoft-Windows-Kernel-Process 
-ot file -p C:\windows\temp\etw.json
```

![[Pasted image 20250520125757.png |500]]

The `etw.json` file (that includes data from the `Microsoft-Windows-Kernel-Process`
provider) seems to contain information about `powershell.exe` being the one who created
`cmd.exe` .

![[Pasted image 20250520125832.png|500]]

It should be noted that SilkETW event logs can be ingested and viewed by Windows Event
Viewer through `SilkService` to provide us with deeper and more extensive visibility into the actions performed on a system.

---
## Detection #2: Malicious .NET Assembly Loading

Traditionally, adversaries employed a strategy known as "Living off the Land" (LotL),
exploiting legitimate system tools, such as PowerShell, to carry out their malicious operations. This approach reduces the risk of detection since it involves the use of tools that are native to the system, and therefore less likely to raise suspicion.



---
## Get-WinEvent

The cmdlet provides us with the capability to retrieve different types of event logs, including classic Windows event logs like System and Application logs, logs generated by Windows Event Log technology, and Event Tracing for Windows (ETW) logs.

```powershell
Get-WinEvent -ListLog * | Select-Object LogName, RecordCount, IsClassicLog, IsEnabled, LogMode, LogType | Format-Table -AutoSize
```
This command provides us with valuable information about each log, including the name of
the log, the number of records present, whether the log is in the classic .evt format or the
newer .evtx format, its enabled status, the log mode (Circular, Retain, or AutoBackup), and
the log type (Administrative, Analytical, Debug, or Operational).

| **Component**   | **Description**                                                            |
| --------------- | -------------------------------------------------------------------------- |
| `-ListLog *`    | Lists **all** available logs on the system, including classic and ETW logs |
| `Select-Object` | Filters and formats output to show only specified properties               |
| `-ListProvider` | Event log providers associated with each log                               |

1. **Retrieving events from the System log**
```powershell
Get-WinEvent -LogName 'System' -MaxEvents 50 |
Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message |
Format-Table -AutoSize
```

2. **Retrieving events from Microsoft-Windows-WinRM/Operational**
	- To retrieve the oldest events, instead of manually sorting the results, we can utilize the `-Oldest` parameter.
```powershell
Get-WinEvent -LogName 'Microsoft-Windows-
WinRM/Operational' -MaxEvents 30 | Select-Object TimeCreated, ID,
ProviderName, LevelDisplayName, Message | 
Format-Table -AutoSize
```
- The following command demonstrates how to retrieve the oldest 30 events from the 'Microsoft-Windows-WinRM/Operational' log.

```powershell
Get-WinEvent -LogName 'Microsoft-Windows-
WinRM/Operational' -Oldest -MaxEvents 30 | Select-Object TimeCreated, ID,
ProviderName, LevelDisplayName, Message | 
Format-Table -AutoSize
```

3. **Retrieving events from .evtx Files**

```powershell
Get-WinEvent -Path 'C:\Tools\chainsaw\EVTX-
ATTACK-SAMPLES\Execution\exec_sysmon_1_lolbin_pcalua.evtx' -MaxEvents 5 |
Select-Object TimeCreated, ID, ProviderName, LevelDisplayName, Message |
Format-Table -AutoSize
```

4. **Filtering events with FilterHashtable**

```powershell
Get-WinEvent -FilterHashtable
@{LogName='Microsoft-Windows-Sysmon/Operational'; ID=1,3} | Select-Object
TimeCreated, ID, ProviderName, LevelDisplayName, Message | 
Format-Table -AutoSize
```

```powershell
Get-WinEvent -FilterHashtable
@{Path='C:\Tools\chainsaw\EVTX-ATTACK-
SAMPLES\Execution\sysmon_mshta_sharpshooter_stageless_meterpreter.evtx';
ID=1,3} | Select-Object TimeCreated, ID, ProviderName, LevelDisplayName,
Message | Format-Table -AutoSize
```

The command above retrieves events with IDs 1 and 3 from the Microsoft-Windows-
Sysmon/Operational event log, selects specific properties from those events, and displays
them in a table format. Note: If we observe Sysmon event IDs 1 and 3 (related to
"dangerous" or uncommon binaries) occurring within a short time frame, it could potentially
indicate the presence of a process communicating with a Command and Control (C2) server.

5. **Filtering events with FilterHashtable & XML**

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'Microsoft-Windows-Sysmon/Operational'
    ID = 3
} | ForEach-Object {
    $xml = [xml] $_.ToXml()
    $eventData = $xml.Event.EventData.Data

    New-Object PSObject -Property @{
        SourceIP     = ($eventData | Where-Object { $_.Name -eq "SourceIp" }      | Select-Object -ExpandProperty '#text')
        DestinationIP= ($eventData | Where-Object { $_.Name -eq "DestinationIp" } | Select-Object -ExpandProperty '#text')
        ProcessGuid  = ($eventData | Where-Object { $_.Name -eq "ProcessGuid" }   | Select-Object -ExpandProperty '#text')
        ProcessId    = ($eventData | Where-Object { $_.Name -eq "ProcessId" }     | Select-Object -ExpandProperty '#text')
    }
} | Where-Object {
    $_.DestinationIP -eq "52.113.194.132"
}
```



---
### References

https://nasbench.medium.com/a-primer-on-event-tracing-for-windows-etw-997725c082bf
https://bmcder.com/blog/a-begginers-all-inclusive-guide-to-etw