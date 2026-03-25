**Containment actions:**

- Isolate Device
- Restrict app execution
- Run antivirus scan

**Investigation actions:**

- Initiate Automated Investigation
- Collect investigation package
- Initiate Live Response Session

---
## Containment Actions
### Isolate devices from networks

- Disconnects the device **from the network**.
- Maintains connectivity to **Microsoft Defender services**, so monitoring continues.
- Optionally allows **select communication apps** (Outlook, Teams, Skype) to function — known as **Selective Isolation** (Windows 10 1709+)
- Device shows a **notification** that it’s isolated.
- View result in the **Action Center** and **Device Timeline**.

### Restrict app execution

- *Only* available on Microsoft Defender AV.
- Blocks **unauthorized or potentially malicious apps** from running on a device.
- Applies a *Code Integrity Policy* (via Windows Defender Application Control).
- Only apps *signed by Microsoft-issued certificates* are allowed to run.
- View result in the *Action Center* & *Device Timeline*.

### Run antivirus scan

 - Selected Run antivirus scan *->* select the scan type (quick or full)
 - View result in the *Action Center* & *Device Timeline*.
 - Microsoft Defender AV *ScanAvgCPULoadFactor* value still applies and limits the CPU impact of the scan. Default value is 50% max. 

---
## Investigation Action

### 1. Initiate automated investigations

- You can **manually start** a general-purpose automated investigation on any device.
- While it runs, **new alerts from that device** will be **added automatically** to the same investigation.
- If the **same threat appears on other devices**, those devices are **included** in the investigation.

### 2. Collect investigation package

Collect forensic data from a device to analyze suspicious activity, tools, and attacker behavior.

**🛠 How to Collect**
- Go to the **Device page** > Click **Collect investigation package**
- Provide a reason > **Confirm**
- Download the *.zip* file from the **Action Center**

#### 📁 What's Inside the ZIP

**🔧 Autoruns**

Registry snapshots of auto-start entries (ASEPs) *->* reveals persistence techniques

**📦 Installed programs**

List of all installed software (.csv)

**🌐 Network connections**

- *ActiveNetConnections.txt*: Current connections
- *Arp.txt*: ARP cache (find other hosts)
- *DnsCache.txt*: DNS resolver cache
- *IpConfig.txt*: Adapter configurations
- *FirewallExecutionLog.txt* / *pfirewall.log*

**🚀 Prefetch**

Recently run programs (even if deleted)

Requires a prefetch viewer

**⚙️ Processes**

List of currently running processes (.csv)

**⏲️ Scheduled tasks**

Auto-executing jobs and scripts (.csv)

**🔐 Security Event Log**

Audit trail of login/logout and other security events

**📋 Services**

Running services + status (.csv)

**📁 SMB Sessions**

SMB inbound/outbound logs *->* lateral movement detection

**🖥️ System Info**

OS version, hardware info

**🧹 Temp Directories**

Temp files for each user

**👤 Users and Groups**

Group membership mappings

**📑 WdSupportLogs**

Support logs (*MpCmdRunLog.txt*, *MPSupportFiles.cab*)

**📊 CollectionSummaryReport.xls**

Summary of all collected data + errors (if any)

### 3. Live Response Session

- Run basic and advanced commands
- Download files such as malware samples and outcomes of PowerShell scripts.
- Download files in the background (new!).
- Upload a PowerShell script or executable to the library and run it on a device from a tenant level.
- Take or undo remediation actions.

### 🛠️ Prerequisites for Live Response Session

1. **Supported OS:** Device must run **Windows 10 or later**.
2. **Enable Live Response**
    - Go to **Advanced features** → Enable **Live Response**.
    - Only users with **Manage Portal Settings** permission can do this.
3. **Automation Remediation Level**
    - The device must be part of a **device group** with at least **minimum remediation level** set.
    - Otherwise, session **cannot be established**.
4. **(Optional) Unsigned Script Execution**
    - Can be enabled in **Advanced features**.
    - ⚠️ Not recommended — increases risk exposure.
5. **Proper RBAC Permissions**
	- Required to **initiate sessions**
	- Required to **upload scripts to library**
	- Limited capabilities with **delegated roles**

#### Live response dashboard overview

- Who created the session
- When the session started
- The duration of the session
- Disconnect session
- Upload files to the library
- Command console
- Command log

__Basic Commands__

|Command|Description|
|---|---|
|[cd]|Changes the current directory.|
|[cls]|Clears the console screen.|
|[connect]|Starts a live response session to the device.|
|[connections]|Shows all the active connections.|
|[dir]|Shows a list of files and subdirectories in a directory.|
|[getfile] <file_path>|Downloads a file in the background.|
|[drivers]|Shows all drivers installed on the device.|
|[fg] <command ID>|Returns a file download to the foreground.|
|[fileinfo]|Get information about a file.|
|[findfile]|Locates files by a given name on the device.|
|[help]|Provides help information for live response commands.|
|[persistence]|Shows all known persistence methods on the device.|
|[processes]|Shows all processes running on the device.|
|[registry]|Shows registry values.|
|[scheduledtasks]|Shows all scheduled tasks on the device.|
|[services]|Shows all services on the device.|
|[trace]|Sets the terminal's logging mode to debug.|

__Advanced Commands__

|Command|Description|
|---|---|
|analyze|Analyses the entity with various incrimination engines to reach a verdict.|
|getfile|Gets a file from the device. This command has a prerequisite command. You can use the -auto command with getfile to automatically run the prerequisite command.|
|run|Runs a PowerShell script from the library on the device.|
|library|Lists files that were uploaded to the live response library.|
|putfile|Puts a file from the library to the device. Files are saved in a working folder and are deleted when the device restarts by default.|
|remediate|Remediates an entity on the device. The remediation action will vary depending on the entity type. This command has a prerequisite command. You can use the -auto command with remediate to automatically run the prerequisite command.|
|Undo|Restores an entity that was remediated.|
#### ⚠️ Live Response Limitations

- ⏱️ **Max sessions:** 10 concurrent live sessions
- 👤 **Per-user limit:** Only 1 session at a time
- 💻 **Per-device limit:** Only 1 session at a time
- 🔌 **Inactivity timeout:** 5 minutes
- 🚫 **No large-scale command execution**

📦 **File Size Limits**

|Operation|Limit|
|---|---|
|`getfile`|3 GB|
|`fileinfo`|10 GB|
|Library file|250 MB max|

**Supported output types**

- *-output json*    
- *-output table*