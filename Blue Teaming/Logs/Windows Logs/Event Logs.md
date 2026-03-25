### Windows Event Logging Basics

- By default, Windows is configured to generate event log entries and store them in a local directory, `C:\Windows\System32\winevt\Logs`, saved as `.evtx` files. Some of these files have generic names (such as Security), while others share the name of the provider that generated them. A provider is the name of a program or driver that writes event data to the event log files.
- Windows event logging offers comprehensive logging capabilities for *application errors, security events, and diagnostic information*. 
- The logs are categorized into different event logs, such as "Application", "System", "Security", and others, to organize events based on their source or purpose.
- Event logs can be accessed using the Event Viewer application or programmatically using APIs such as the `Windows Event Log API`.

Accessing the Windows Event Viewer as an administrative user allows us to explore the
various logs available.

>**Note** that the Windows Event Viewer has the ability to open and display previously saved `.evtx` files, which can be then found in the "Saved Logs" section.

---
## Event Log Viewer
### Windows Logs

| **Log Name**         | **Description**                                                                 | **Examples**                                              |
| -------------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------- |
| **Application**      | Events from Windows-based applications.                                         | App crashes, hangs, .NET runtime errors.                  |
| **Security**         | Security and audit-related events.                                              | Logon attempts, object access, policy changes.            |
| **Setup**            | Events related to system installation or Windows Update.                        | OS upgrades, feature installs.                            |
| **System**           | Logs from core OS components not fitting other categories.                      | Driver load failures, reboot reasons, service start/stop. |
| **Forwarded Events** | Events collected from remote computers (used in centralized logging scenarios). | Remote system logs, domain controller audits.             |

### Application and Services Logs

| **Log Name**                      | **Category**                 | **Description**                                                                 | **Examples**                            |
| --------------------------------- | ---------------------------- | ------------------------------------------------------------------------------- | --------------------------------------- |
| **Application and Services Logs** | Custom/Service-specific Logs | Fine-grained logs for services or applications from Microsoft or third parties. | DNS Server, Windows PowerShell, Sysmon. |

---
### Event primary components

|**Field**|**Description**|
|---|---|
|**Log Name**|The name of the event log (e.g., Application, System, Security, etc.).|
|**Source**|The software or component that logged the event.|
|**Event ID**|A unique identifier for the event.|
|**Task Category**|A category or label describing the task that generated the event.|
|**Level**|The severity of the event (Information, Warning, Error, Critical, Verbose).|
|**Keywords**|Flags used to categorize events (e.g., "Audit Success", "Audit Failure").|
|**User**|The user account that was logged on when the event occurred.|
|**OpCode**|Indicates the specific operation the event represents.|
|**Logged**|The date and time when the event was logged.|
|**Computer**|The name of the computer where the event occurred.|
|**XML Data**|A structured XML representation of all the above information, including additional event-specific details.|

>The **Keywords** field is particularly useful when filtering event logs for specific types of events. It can significantly enhance the precision of search queries by allowing us to specify events of interest.

---
### 📊 Event Log Levels

| **Level**         | **Description**                                                                                 | **Example Event Types**                          |
| ----------------- | ----------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| **Critical**      | Indicates a serious problem or system failure that requires immediate attention.                | System crash, service failure, disk failure      |
| **Error**         | Signifies a significant issue, such as failure of a component or an operation.                  | Failed logon, application crash, hardware error  |
| **Warning**       | Indicates a potential issue or something unexpected, but not necessarily harmful.               | Low disk space, high memory usage, network delay |
| **Information**   | Confirms that an operation or service completed successfully or is functioning as expected.     | Service started, user logged on, task created    |
| **Verbose**       | Provides detailed tracing information primarily for diagnostic purposes.                        | Debugging details, extended logs                 |
| **Audit Success** | (Security log only) Indicates a successful security-related operation (e.g., successful logon). | Logon success, permission granted                |
| **Audit Failure** | (Security log only) Indicates a failed security-related operation (e.g., failed login attempt). | Failed logon, denied access                      |

> ### 📌 Notes: 
> - **Critical**, **Error**, and **Warning** usually deserve the most attention in **System** and **Application** logs.
> - **Audit Success** and **Audit Failure** are **unique to Security logs** and focus on login/authentication and permissions.
> - **Verbose** logs are **only available** if verbose logging is explicitly enabled and are useful mainly for developers or admins during deep diagnostics.

---
## Useful Windows Event Logs

### 🖥️ System Logs

| ***Event ID*** | ***Description***                 | ***Significance***                                                           |
| -------------- | --------------------------------- | ---------------------------------------------------------------------------- |
| 1074           | System shutdown/restart initiated | Detect unexpected reboots, possible malware or unauthorized user activity    |
| 6005           | Event Log service started         | Marks system startup; useful for boot-time analysis and incident correlation |
| 6006           | Event Log service stopped         | Marks system shutdown; unexpected stoppage may indicate tampering            |
| 6013           | Windows uptime (in seconds)       | Daily event; short uptimes may indicate reboots and potential intrusions     |
| 7040           | Service startup type changed      | Indicates service config tampering (manual ↔ auto); a sign of persistence    |

### 🔐 Security Logs

| ***Event ID*** | ***Description***                                  | ***Significance***                                                             |
| -------------- | -------------------------------------------------- | ------------------------------------------------------------------------------ |
| 1102           | Audit log cleared                                  | Strong indicator of log tampering; often used to hide malicious activity       |
| 1116           | Antivirus detected malware                         | Malware detection by Defender; may indicate infection                          |
| 1118           | Antivirus remediation started                      | Shows that Defender began remediation                                          |
| 1119           | Antivirus remediation succeeded                    | Indicates malware was successfully removed/quarantined                         |
| 1120           | Antivirus remediation failed                       | Failed cleanup attempt; high-priority alert                                    |
| 4624           | Successful logon                                   | Used for user behavior analysis; look for anomalies                            |
| 4625           | Failed logon                                       | Detects brute-force or unauthorized access attempts                            |
| 4648           | Logon with explicit credentials                    | May indicate lateral movement by attackers                                     |
| 4656           | Handle requested for object (file, registry, etc.) | Detects access attempts to sensitive resources                                 |
| 4672           | Privileged logon                                   | Detects use of superuser/admin-level privileges                                |
| 4698           | Scheduled task created                             | Attackers often use tasks for persistence                                      |
| 4700/4701      | Scheduled task enabled/disabled                    | Tracks manipulation of existing tasks                                          |
| 4702           | Scheduled task updated                             | Indicates changes to task (could be malicious modifications)                   |
| 4719           | Audit policy changed                               | Disabling audit logs could signal tampering                                    |
| 4738           | User account changed                               | Tracks privilege or settings changes to user accounts                          |
| 4771           | Kerberos pre-authentication failed                 | Detects Kerberos-based brute-force attacks                                     |
| 4776           | Domain controller credential validation attempt    | Monitors login validation attempts; failures may indicate brute-force attempts |
| 5001           | Defender real-time protection config changed       | Could indicate attempt to weaken antivirus protection                          |
| 5140           | Network share accessed                             | Useful for detecting unauthorized file access                                  |
| 5142           | Network share created                              | Unauthorized shares may indicate exfiltration attempt                          |
| 5145           | Network share access attempt                       | Repeated attempts may indicate enumeration of shares                           |
| 5157           | Windows Filtering Platform blocked connection      | Can reveal blocked malicious network traffic                                   |
| 7045           | A service was installed on the system              | Could indicate malware persistence via rogue services                          |

---
## 🔐 Windows Logon Types – Event ID 4624 / 4625

|**Logon Type**|**Description**|**Use Case / Context**|**Common Threat Use**|
|---|---|---|---|
|**2**|Interactive|User logged on at console (keyboard/monitor).|Physical access|
|**3**|Network|Logon via SMB, RDP, HTTP, etc. (no session created).|Lateral movement, brute-force over network|
|**4**|Batch|Scheduled task or script runs under a user context.|Living off the land / scheduled persistence|
|**5**|Service|Service startup using a user account.|Privilege escalation via service abuse|
|**7**|Unlock|Workstation unlock with valid credentials.|Post-intrusion credential use|
|**8**|NetworkCleartext|Logon with plaintext password (e.g., Basic auth, IIS, RDP).|Credential theft, password sniffing|
|**9**|NewCredentials (RunAs)|`RunAs` or impersonation used — credentials passed explicitly|Privilege escalation / stealthy movement|
|**10**|RemoteInteractive|RDP session logon (also reports as type 3 and 10).|Lateral movement via RDP|
|**11**|CachedInteractive|Logon using cached domain credentials (offline).|Laptop usage, credential stuffing|