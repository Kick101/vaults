Microsoft Defender for Identity (formerly Azure Advanced Threat Protection, also known as Azure ATP) is a *cloud-based* security solution. 

>Defender for identity uses your *on-premises Active Directory* signals to identify, detect, and investigate advanced threats, compromised identities, and malicious insider actions directed at your organization. 

**MDI benefits:**

- Monitor users, entity behavior, and activities with learning-based analytics
- Protect user identities and credentials stored in Active Directory
- Identify and investigate suspicious user activities and advanced attacks throughout the kill chain
- Provide clear incident information on a simple timeline for fast triage

### Process flow for Defender for Identity

![Diagram of the data flow for protecting identities using Microsoft Defender for Identity.](https://learn.microsoft.com/en-us/training/wwl-sci/manage-azure-active-directory-identity-protection/media/defender-identity-topology.png)

### MDI Components

1. **Defender for Identity Portal**  
    Use this portal to **set up**, **view data**, and **investigate threats** detected by Defender for Identity.
    
2. **Defender for Identity Sensor**  
    A lightweight agent installed on:
    - **Domain Controllers** – Monitors traffic and activity directly (no need for port mirroring).
    - **AD FS Servers** – Monitors authentication and network traffic.
        
3. **Defender for Identity Cloud Service**
    - Runs on **Azure infrastructure** (in US, Europe, and Asia).
    - Connects to Microsoft's **Intelligent Security Graph** to enhance detection with global threat intelligence.

---
### 🛡️ Behavioral Detection and User Profiling

Microsoft Defender for Identity monitors and analyzes user behavior across your network to detect advanced threats, compromised identities, and insider threats.

**Behavioral Baseline**:  
- Builds a profile of normal user behavior based on:
- Group memberships
- Access patterns
- Permissions
- Device usage
    
**Adaptive Intelligence**:  

- Anomalies (unusual logins, lateral movement, privilege escalation)
- Risky behavior indicating compromise
- Insider threats

### 🥷 Reduce Attack Surface

- MDI's *visual Lateral Movement Paths* help you quickly understand exactly how an attacker can move laterally inside your organization.
- *MDI security reports* help identify users and devices that authenticate using clear-text passwords and provide additional insights to improve your organizational security posture and policies.

### 🧠 Threat Detection Across Kill Chain

| **Kill Chain Stage** | **Attack Technique**         | **Detection by Defender for Identity**                         |
| -------------------- | ---------------------------- | -------------------------------------------------------------- |
| Reconnaissance       | LDAP enumeration             | Alerts on suspicious LDAP queries targeting sensitive groups   |
| Credential Access    | Password spray / Brute force | Detects Kerberos/NTLM auth failures, spray patterns            |
| Lateral Movement     | Pass-the-ticket              | Alerts when the same Kerberos ticket is reused across machines |
| Domain Dominance     | DCShadow                     | Detects rogue DC registration & replication behavior           |

### 🔎 **Investigation Tools Provided**

- **Alert Details:** Title, description, related users/devices, and commands executed.
- **Evidence View:** Downloadable Excel reports and activity logs.
- **Cross-portal visibility:** Alerts also visible in Microsoft Defender for Cloud Apps.

**Each MDI security alert includes:**

- **Alert title.** Official Microsoft Defender for Identity name of the alert.
- **Description.** Brief explanation of what happened.
- **Evidence.** Additional relevant information and related data about what happened to help in the investigation process.
- **Excel download.** Detailed Excel download report for analysis

![[Pasted image 20250704125026.png]]

**Alerts can also be viewed within MDCA**

![[Pasted image 20250704125037.png]]

[More info ... ](https://learn.microsoft.com/en-us/training/modules/m365-threat-safeguard/review-compromised-accounts)

---
## 🔗 Integration Architecture

### 🌐 Cloud Service

- **Runs on Azure** (US, EU, Asia)
- Connected to **Microsoft Intelligent Security Graph**


## 🤝 Integrations

### 1. Microsoft Defender for Cloud Apps (MDCA)

- View on-premises user activities.
- Correlates **alerts and suspicious behavior** across **cloud + on-prem** environments.
- MDI policies show up in **MDCA policy interface**.

### 2. **Microsoft Defender for Endpoint (MDE)**

- MDI: Monitors **Domain Controller traffic**
- MDE: Monitors **endpoints**
- Integration enables:
    - Viewing MDI alerts directly in the **MDE portal**
    - A unified threat view across identity and endpoints

### 🔍 Example Use Case

- MDE raises a **high severity malware alert**.
- MDI confirms a **Pass-the-Hash attack** using **Mimikatz**.
- Analysts review a **timeline of credential theft** events via Defender portals.

### 🧠 Analyst Benefits

- Combined view = better **incident correlation**
- Timeline of events improves **attack investigation**
- Integrated threat signals help build complete **attack narratives**
