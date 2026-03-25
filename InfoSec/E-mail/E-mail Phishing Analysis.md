## Phishing Analysis Methodology

- [[#📥 User Reporting & Triage Tools]]
- Header & Sender examination
- Content examination (grammar etc)
- [[#🌐 URL & Domain Analysis Tools]]
- [[#🔬 Sandbox & Analysis Tools]]
- Contextual examination
- Defense measures (reactive, proactive) 
- Documentation & Reporting

---
## Collecting Artifacts

```
sender address:
subject:
from address:
timestamp:
recipients:
reply-to-address:
urls:
attachements:
```



---
## 🔍 Email Security & Gateway Solutions

| Tool                                      | Description                                                              |
| ----------------------------------------- | ------------------------------------------------------------------------ |
| **Proofpoint**                            | Advanced threat protection, URL rewriting, phishing detection with ML.   |
| **Mimecast**                              | Email security with targeted threat protection, impersonation detection. |
| **Microsoft Defender for Office 365**     | Cloud-native phishing protection, safe links, safe attachments.          |
| **Google Workspace Security (Gmail ATP)** | Built-in scanning for phishing and malware in Gmail.                     |
| **Barracuda Email Security**              | Anti-phishing, spear phishing, and impersonation protection.             |
| **Cisco Secure Email (IronPort)**         | Enterprise-grade email threat detection and anti-spam.                   |

## 🧠 Machine Learning / Behavioral Detection Tools

| Tool                  | Description                                                                   |
| --------------------- | ----------------------------------------------------------------------------- |
| **IRONSCALES**        | AI-driven phishing detection with real-time user feedback loop.               |
| **GreatHorn**         | Contextual analysis and adaptive risk scoring for emails.                     |
| **Abnormal Security** | ML-based detection of social engineering and business email compromise (BEC). |
| **Area 1 Security**   | Preemptive phishing protection using predictive models.                       |

## 🔬 Sandbox & Analysis Tools

| Tool                | Description                                                      |
| ------------------- | ---------------------------------------------------------------- |
| **Cuckoo Sandbox**  | Open-source malware analysis system.                             |
| **Any.Run**         | Interactive sandbox for real-time malware and phishing behavior. |
| **Joe Sandbox**     | Deep file and link analysis with static/dynamic techniques.      |
| **Hybrid Analysis** | Malware sandbox with community threat scoring.                   |

## 🌐 URL & Domain Analysis Tools

|Tool|Description|
|---|---|
|**VirusTotal**|Multi-engine scanner for URLs, files, and domains.|
|**URLscan.io**|Visualizes and records web page behavior for phishing or scams.|
|**PhishTool**|Purpose-built platform for URL, domain, and header analysis.|
|**WHOIS / DNS Tools**|E.g., `whois`, `dig`, RiskIQ, DomainTools.|
## 🛡️ Threat Intelligence & IOC Enrichment

| Tool                                            | Description                                               |
| ----------------------------------------------- | --------------------------------------------------------- |
| **MISP (Malware Information Sharing Platform)** | Open-source platform for threat intel sharing.            |
| **AlienVault OTX**                              | Community-powered threat intelligence feed.               |
| **Anomali ThreatStream**                        | Commercial TI platform integrated with detection systems. |
| **AbuseIPDB**                                   | Public IP reputation database.                            |
| **Cisco Talos Intelligence**                    | Provides domain/IP threat intelligence.                   |


## 🧰 SIEM & Log Correlation Tools

|Tool|Description|
|---|---|
|**Splunk (Phantom)**|Custom detection rules, SOAR capabilities.|
|**ELK Stack (Elasticsearch, Logstash, Kibana)**|Open-source logging and threat detection.|
|**IBM QRadar**|Prebuilt phishing rules and correlation for email logs.|
|**Microsoft Sentinel**|Cloud-native SIEM with Office 365 and Defender integration.|

## 📥 User Reporting & Triage Tools

| Tool                          | Description                                                 |
| ----------------------------- | ----------------------------------------------------------- |
| **PhishER by KnowBe4**        | Automated analysis and triage platform for reported emails. |
| **Cofense Reporter / Triage** | Enables end-user reporting + analyst triage workflows.      |
| **GoPhish (Red Teaming)**     | Simulates phishing campaigns for detection readiness.       |

## 📊 Phishing Detection Tools – Comparison Matrix

|Tool / Platform|Category|Core Features|Integration Ease|Pricing Model|Best For|
|---|---|---|---|---|---|
|**Microsoft Defender for O365**|Email Security Gateway|Safe links/attachments, real-time alerts, threat hunting|Native to M365|License-based|Enterprise, M365 environments|
|**Proofpoint TAP**|Email Security Gateway|Targeted threat protection, phishing kits detection, URL rewriting|Moderate (API available)|Subscription|Large orgs with layered defense|
|**Mimecast**|Email Security Gateway|Impersonation protection, content control, threat dashboard|Moderate|Subscription|Compliance-heavy industries|
|**IRONSCALES**|AI + User Feedback|ML-based analysis, real-time phishing response, user-assisted triage|Easy (cloud-native)|Tiered pricing|SMBs to Enterprises|
|**Abnormal Security**|Behavioral/ML Detection|Behavioral anomaly detection, executive impersonation detection|Easy (API-based)|Premium|High-value targets (C-Suite)|
|**GreatHorn**|Risk Scoring + Automation|Role-based context, automated remediation, Slack/Teams alerts|Easy|Tiered subscription|Dev-centric or fast-moving teams|
|**Cuckoo Sandbox**|Sandbox|Dynamic file and attachment analysis (open source)|Moderate (manual setup)|Free/Open-source|Labs, SOCs, malware analysts|
|**Any.Run**|Sandbox|Interactive malware behavior analysis with visual trace|Easy (Web UI)|Freemium|Rapid detonation + demos|
|**Joe Sandbox**|Sandbox|Deep static + dynamic analysis with ML scoring|Complex|Paid (per use)|Advanced threat research|
|**VirusTotal**|URL/File Analysis|60+ AV engines, passive DNS, behavioral insights|Easy (API available)|Freemium|IOC enrichment|
|**URLscan.io**|URL Behavior Analysis|Web page rendering + sandbox, redirects, phishing template detection|Easy|Freemium/API tiers|SOCs, IR teams|
|**PhishTool**|Analyst Workflow|Header decoding, auto-analysis, IOC extraction|Moderate|Paid|Analyst-first phishing triage|
|**AlienVault OTX**|Threat Intel Feed|Community IOCs, threat actor TTPs, pulse sharing|Easy|Free|IOC enrichment for SOCs|
|**MISP**|Threat Intel Platform|Structured threat intel sharing, STIX/TAXII|Moderate (self-host)|Open-source|CTI teams, national CSIRTs|
|**Splunk + Phantom**|SIEM + SOAR|Custom detection rules, automated playbooks, phishing triage|Complex|Paid, scalable|Large SOCs|
|**Microsoft Sentinel**|SIEM + Cloud Analytics|KQL queries, Office 365 telemetry, UEBA|Easy (cloud-native)|Pay-as-you-go|Cloud-native detection|
|**Cofense Triage/Reporter**|Reporting + Analysis|User phishing reports, analyst dashboard, IOC tagging|Moderate|Paid|Awareness + IR integration|
|**GoPhish**|Simulation & Detection|Open-source phishing simulation platform|Easy|Free|Red teams, user training|

## 🧩 Integration Examples
| Stack                  | Tools                                            | Notes                                            |
| ---------------------- | ------------------------------------------------ | ------------------------------------------------ |
| **Microsoft-based**    | Microsoft Defender, Sentinel, OTX, VirusTotal    | Seamless M365-native experience.                 |
| **Linux SOC Stack**    | MISP, GoPhish, ELK, Cuckoo, PhishTool            | Full open-source capability, high customization. |
| **Hybrid/Cloud-first** | IRONSCALES, Abnormal, Splunk Phantom, VirusTotal | Fast deployment, auto-scaling, great for MSSPs.  |

## 🔁 Suggested SOAR Workflow (Example)

- **Input**: User reports via Outlook > Defender > PhishER or Cofense.
- **Triage**: Automated header & URL analysis → VirusTotal + URLscan.
- **Classification**: Heuristics + ML scoring → confidence %.
- **Action**: Remove from inbox → block indicators via firewall/DNS.
- **Post-Processing**: Auto-document in MISP or case mgmt.
- **Report/Train**: Weekly phishing report → training updates via GoPhish.

---
## Auto Response
- Triage incoming phishing reports.
- Enrich with threat intel (VirusTotal, AbuseIPDB).
- Auto-quarantine messages from inboxes (via O365/Google Workspace APIs).
- Block malicious IOCs across network perimeter.
- Auto-notify users with results and advisories.
- Escalate to human analyst if high severity or uncertain classification.

---
## Attacker Frameworks

- **Phishing Kits**
    - Off-the-shelf kits (e.g., 16Shop, Evilginx, LogoKit) for credential harvesting.
- **C2 Frameworks**    
    - **Cobalt Strike**, **Empire**, **Mythic** used post-compromise.
- **Email Spoofing Tools**
    - **SendEmail**, **GoPhish**, **King Phisher**, **Phishery**.
- **Social Engineering TTPs**
    - Credential harvesting, malware delivery (droppers), lateral movement via mailbox access.
- **Malware Payloads**
    - Embedded macros (e.g., Agent Tesla, Emotet), links to loaders, or ransomware droppers.