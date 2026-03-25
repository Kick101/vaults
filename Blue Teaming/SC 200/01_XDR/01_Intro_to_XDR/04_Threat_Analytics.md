# Analyze threat analytics

![[Pasted image 20250702144016.png]]

**Microsoft Threat Analytics** is a **threat intelligence solution** built into Microsoft Defender XDR. It helps SOC teams detect, assess, and respond to:

- Active threat actors & campaigns
- Attack techniques
- Critical vulnerabilities
- Malware
- Common attack surfaces

### 📊 Threat Analytics Dashboard

- **Microsoft Defender portal > Threat Intelligence**
- **Threat Analytics card** on the homepage

#### Key Dashboard Sections:

|Section|Description|
|---|---|
|Latest threats|Most recently published/updated reports|
|High-impact threats|Based on number/severity of alerts/incidents|
|Highest exposure|Based on vulnerability severity & number of exploitable devices|

### 🏷 Filtering & Tags

| Filter Type  | Examples                                            |
| ------------ | --------------------------------------------------- |
| Threat Tags  | Ransomware, Phishing, Vulnerability, Activity group |
| Report Types | Tools & Techniques, Malware, Campaigns              |

### 🧠 Analyst Reports

- Maps **attack stages** using the **MITRE ATT&CK** framework
- Lists **hunting queries**, **IOC patterns**, and **recommendations**

### 🛡 **Organizational Impact & Posture**

|Metric|Meaning|
|---|---|
|Related incidents|Number/severity of linked alerts/incidents|
|Alerts over time|Trends in alert generation/resolution|
|Impacted assets|Devices/mailboxes with unresolved alerts|
|Secure config status|Devices lacking recommended settings|
|Patch status|Devices missing vulnerability fixes|

### 📧 Email Notifications for Report Updates

1. Go to **Settings > Email notifications > Threat analytics**
2. Click + Create notification rule
3. Configure:
    - Rule name
    - Report tags/types
    - Recipients
4. Save and test notification

>⚠️ _Note_: Rule names must be alphanumeric (no spaces or punctuation)


---
# Analyze reports

The Reports blade in the Microsoft Defender portal provides access to all the available reports for Microsoft Defender for Endpoint and Microsoft Defender for Office 365.

## General

|Name|Description|
|---|---|
|Security report|View information about security trends and track the protection status of your identities, data, devices, apps, and infrastructure.|

## Endpoints

|Name|Description|
|---|---|
|Threat protection|See details about the security detections and alerts in your organization.|
|Device health and compliance|Monitor the health state, antivirus status, operating system platforms, and Windows 10 versions for devices in your organization.|
|Vulnerable devices|View information about the vulnerable devices in your organization, including their exposure to vulnerabilities by severity level, exploitability, age, and more.|
|Web protection|Get information about the web activity and web threats detected within your organization.|
|Firewall|View connections blocked by your firewall including related devices, why they were blocked, and which ports were used|
|Device control|This report shows your organization's media usage data.|
|Attack surface reduction rules|View information about detections, misconfiguration, and suggested exclusions in your environment.|

## Email & Collaboration

|Name|Description|
|---|---|
|Email & collaboration reports|Review Microsoft recommended actions to help improve email and collaboration security.|
|Manage schedules|Manage the schedule for the reports security teams use to mitigate and address threats to your organization.|
|Reports for download|Download one or more of your reports.|
|Exchange mail flow reports|Deep link to Exchange mail flow report in the Exchange admin center.|


