
|Functional Area|Weight|Key Focus Areas|
|---|---|---|
|**Manage a security operations environment**|20–25%|Configure SIEM/SOAR, Defender XDR, asset exposure, ingestion, and environment management|
|**Configure protections and detections**|15–20%|Configure policies, rules, deception, threat detection in Defender + Sentinel|
|**Manage incident response**|25–30%|Investigate/respond to alerts in Defender portal and Sentinel, live response, playbooks|
|**Manage security threats**|15–20%|Threat hunting using KQL, threat indicators, ATT&CK mapping, custom hunts, workbooks|

---
### 🔧 1. Manage a Security Operations Environment (20–25%)

> Focuses on initial setup, workspace and environment configuration, role-based access, log ingestion.

**Tools:**
- Microsoft Sentinel (workspace, RBAC, data connectors, CEF/Syslog, custom tables)
- Microsoft Defender for Endpoint (automation levels, device groups, exposure management)
- Microsoft Defender for Cloud (discover unprotected resources)
- Microsoft 365 & Azure RBAC

### 🛡️ 2. Configure Protections and Detections (15–20%)

> Focuses on implementing and tuning detection/prevention logic.

**Tools:**

- **Defender for Endpoint** (ASR, attack surface policies)
- **Defender for Office 365** (anti-phishing, DLP)
- **Defender for Cloud Apps**
- **Sentinel** (Analytics Rules, ASIM parsers, behavioral analytics)
- **Microsoft Defender XDR** (custom detection, deception rules)

### 🧯 3. Manage Incident Response (25–30%)

> Focuses on triaging and responding to real incidents.

**Tools:**

- **Microsoft Sentinel** (incidents, playbooks, automation)
- **Microsoft Defender XDR** (timeline, remediation actions)
- **Microsoft Defender for Cloud** (workload protection)
- **Microsoft Entra ID, Purview, Identity Protection**
- **Security Copilot** (prompts, integrations, incident investigation)

### 🎯 4. Manage Security Threats (15–20%)

> Focuses on threat hunting, TTP analysis, use of threat intelligence.

**Tools:**

- **Microsoft Sentinel** (hunting queries, bookmarks, archived logs, ATT&CK coverage)
- **Microsoft Defender XDR** (threat analytics, hunting with KQL)
- **Security Copilot**
- **KQL mastery** is core

---
## 1. Sentinel

| Learning Path                                                                                                                                                                               | Progress |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| ✅ [Configure your Microsoft Sentinel environment](https://learn.microsoft.com/en-us/training/paths/sc-200-configure-azure-sentinel-environment/)                                            | Done     |
| ✅ [Connect logs to Microsoft Sentinel](https://learn.microsoft.com/en-us/training/paths/sc-200-connect-logs-to-azure-sentinel/)                                                             | Done     |
| ✅ [Create detections and perform investigations using Microsoft Sentinel](https://learn.microsoft.com/en-us/training/paths/sc-200-create-detections-perform-investigations-azure-sentinel/) | Done     |
| ✅ [Perform threat hunting in Microsoft Sentinel](https://learn.microsoft.com/en-us/training/paths/sc-200-perform-threat-hunting-azure-sentinel/)                                            | Done     |
| ✅ [Create queries for Microsoft Sentinel using Kusto Query Language (KQL)](https://learn.microsoft.com/en-us/training/paths/sc-200-utilize-kql-for-azure-sentinel/)                         | Done     |

## 2. Defender XDR

| Learning Path                                                                                                                                                                       | Progress              |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| ✅ [SC-200: Mitigate threats using Microsoft Defender XDR](https://learn.microsoft.com/en-us/training/paths/sc-200-mitigate-threats-using-microsoft-365-defender/)                   | Done                  |
| ✅ [SC-200: Mitigate threats using Microsoft Defender for Endpoint](https://learn.microsoft.com/en-us/training/paths/sc-200-mitigate-threats-using-microsoft-defender-for-endpoint/) | incomplete            |
| ✅ [SC-200: Mitigate threats using Microsoft Purview](https://learn.microsoft.com/en-us/training/paths/sc-200-mitigate-threats-using-microsoft-purview/)                             |                       |
| ✅ [SC-200: Mitigate threats using Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/training/paths/sc-200-mitigate-threats-using-azure-defender/)                     | incomplete (aws, gcp) |
| ✅ [SC-200: Mitigate threats using Microsoft Security Copilot](https://learn.microsoft.com/en-us/training/paths/sc-200-mitigate-threats-using-microsoft-copilot-for-security/)       |                       |

