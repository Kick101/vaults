## Core Technologies

### 1. Behavioral Sensors
- Built into Windows 10 and newer.
- Collect behavioral data from the OS.
- Send data to Defender’s cloud.

### 2. Cloud Analytics
- Uses big data and machine learning.
- Converts behavior into detections, insights, and responses.

### 3. Threat Intelligence
- Provided by Microsoft and partners.
- Identifies attacker tools, techniques, and alerts based on observed behavior.

---
# Capabilities

## 🔍 Threat and vulnerability management

**Covers gaps across SecOps and IT by:**

- Correlating EDR with endpoint vulnerabilities
- Linking machine exposure data (vulns + configs)
- Enabling built-in remediation via Intune & Endpoint Manager

## 🛡️ Attack surface reduction

*Ensures configuration settings are properly set and exploit mitigation techniques are applied.*

- **Hardware-based isolation** ensures system integrity during boot and runtime; isolates Edge browser via containers.
- **Application control** allows only trusted apps to run, rejecting unknowns by default.
- **Exploit protection** applies mitigation techniques to apps org-wide.
- **Network protection** blocks malicious traffic beyond just Edge browser.
- **Controlled folder access** defends key system folders from unauthorized changes (e.g. ransomware).
- **Attack surface reduction** blocks common malware vectors like scripts, Office files, and email.
- **Network firewall** filters inbound/outbound traffic at the host level.

## 💡 Next generation protection

**Microsoft Defender Antivirus** is a built-in *antimalware* solution offering next-gen protection for desktops, laptops, and servers.

- **Cloud-delivered protection** for rapid threat detection and blocking using machine learning and Microsoft’s Intelligent Security Graph.
- **Always-on scanning** (real-time protection) with behavior monitoring and heuristics.
- **Frequent protection updates** powered by machine learning, big data analysis, and threat research.

## 🎯 Endpoint detection and response

- **Alerts** are grouped into **incidents** based on shared attack techniques or attacker attribution — simplifying investigation and response.
- Based on the **"assume breach"** model:
    - Continuously collects **behavioral telemetry**:
        - Process & network activity
        - Kernel & memory insights
        - User sign-ins
        - Registry and file system changes
- **Telemetry is stored for** *6 months*, enabling historical investigation and pivoting across multiple dimensions.
- **Security Operations Dashboard** gives a high-level view of detections and required response actions.

## 🤖 Automated investigation and remediation

Microsoft Defender for Endpoint reduces alert fatigue using **automated investigation and remediation**:

- ✅ **Broad visibility** across multiple endpoints leads to high alert volume.
- 🔄 **Automated investigation** uses:
    - Inspection algorithms
    - Analyst-style playbooks
- ⚙️ These processes:
    - Analyze alerts
    - Take **automatic remediation actions**
    - Free up security teams to focus on **advanced threats**

**Example**: Malware is detected and remediated automatically — reducing manual workload.

