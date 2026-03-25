## 🔐 Identity & Access Policies (Microsoft Entra ID / Identity Protection)

| **Policy Type**               | **Purpose**                                                        |
| ----------------------------- | ------------------------------------------------------------------ |
| **Conditional Access**        | Control access to apps based on risk, user, device, and location.  |
| **User Risk Policy**          | Block or allow users based on Identity Protection user risk level. |
| **Sign-in Risk Policy**       | Evaluate risk during login and enforce MFA or block access.        |
| **MFA Policies**              | Enforce multifactor authentication for critical users or actions.  |
| **Token Protection Policy**   | Bind tokens to devices to prevent token theft.                     |
| **Session Lifetime Policies** | Limit token lifetimes and enforce sign-in frequency.               |

## 💻 Endpoint Security Policies (Intune + Defender for Endpoint)

|**Policy Type**|**Purpose**|
|---|---|
|**Antivirus Policies**|Manage Defender AV settings, exclusions, and cloud protection.|
|**Firewall Rules**|Configure network access and port control on endpoints.|
|**Endpoint Detection & Response (EDR) Policies**|Enable sensor settings, response automation.|
|**Attack Surface Reduction Rules (ASR)**|Block known threat vectors like macro abuse, scripts, etc.|
|**BitLocker Encryption Policies**|Enforce device disk encryption.|
|**Device Compliance Policies**|Determine if devices meet minimum requirements for access.|
|**Remediation Policies (AIR)**|Configure automated or semi-automated threat remediation.|

## 📧 Email & Collaboration Policies (Defender for Office 365)

|**Policy Type**|**Purpose**|
|---|---|
|**Anti-Phishing Policies**|Detect impersonation and spoofing attempts.|
|**Anti-Spam Policies**|Filter out junk mail and known spam threats.|
|**Anti-Malware Policies**|Scan email attachments and block known malicious files.|
|**Safe Links & Safe Attachments**|Real-time scanning and detonation of URLs and files.|
|**Quarantine Policies**|Manage how users interact with quarantined items.|
|**Mailbox Intelligence Policies**|Detect anomalies based on mailbox usage patterns.|

## 🌐 Cloud App Security Policies (Defender for Cloud Apps)

|**Policy Type**|**Purpose**|
|---|---|
|**Access Policies**|Define real-time conditions to allow or block access.|
|**Session Policies**|Control in-app actions: download, copy, paste, print.|
|**Activity Policies**|Trigger alerts based on unusual user or admin behavior.|
|**File Policies**|Detect sensitive files and apply protection.|
|**OAuth App Policies**|Monitor risky third-party app permissions.|

## 🛡️ Defender for Identity Policies

|**Policy Type**|**Purpose**|
|---|---|
|**Lateral Movement Path Policies**|Detect exposure paths to high-value accounts.|
|**Sensitive Account Policies**|Monitor privileged users for anomalies.|
|**Custom Detection Policies**|Trigger alerts based on behavior patterns in AD.|

## ☁️ Cloud Security Posture Management (Defender for Cloud)

| **Policy Type**                       | **Purpose**                                                |
| ------------------------------------- | ---------------------------------------------------------- |
| **Security Recommendations Policies** | Auto-assess security configurations and recommend fixes.   |
| **Regulatory Compliance Policies**    | Map configurations to compliance standards like CIS, NIST. |
| **Just-in-Time Access (JIT)**         | Limit VM access exposure.                                  |
| **Adaptive Network Hardening**        | Restrict NSGs based on machine learning.                   |
| **Defender Plans Configuration**      | Enable threat protection per resource type.                |

## 📂 Data Protection Policies (Microsoft Purview)

|**Policy Type**|**Purpose**|
|---|---|
|**Sensitivity Labels & Label Policies**|Classify and protect documents and emails.|
|**DLP Policies**|Prevent leaks by monitoring and blocking sensitive data transfer.|
|**Information Barriers**|Restrict communication between groups (e.g., compliance).|
|**Records Management Policies**|Apply retention or deletion based on content type.|
|**Audit & Insider Risk Policies**|Monitor and alert on risky user actions.|


