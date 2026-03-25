## Configure advanced features

The **Advanced features** page in the **General** area of the **Settings - Endpoints** menu of the Defender portal provides the following alert and detection-related settings.

The Advanced features area in General Settings area provides many an on/off switch for features within the product. 

**The following are settings that are alert focused.**

|Feature|Description|
|---|---|
|Live Response|Allows users with appropriate RBAC permissions to investigate devices that they're authorized to access, using a remote shell connection.|
|Live Response unsigned script execution|Enables using unsigned scripts in Live Response.|
|Custom network indicators|Configures devices to allow or block connections to IP addresses, domains, or URLs in your custom indicator lists.|

---

## 📧 Create Alert Notification Rules

> Only *Global Admins* can notify for *all* devices.
> Device groups must be created and managed separately.
> Ensure your SMTP or mail relay accepts Microsoft security emails.

1. **Go to:**
   - `Defender portal > Settings > Endpoints > Email notifications`

2. **Click:** `+ Add item`

3. **Configure General Information:**
   - **Rule name** – Descriptive name for the rule.
   - **Include organization name** – Adds your customer name to the email.
   - **Include tenant-specific portal link** – Adds tenant-specific portal URL.
   - **Include device information** – Adds device name in alert email body.
   - **Devices** – Choose:
     - All devices *(Global Admin only)*
     - Selected device groups *(based on RBAC)*
   - **Alert severity** – Choose alert levels: Low, Medium, High.

4. **Add Notification Recipients:**
   - Enter email addresses.
   - Click `Add recipient` (repeat for multiple).
   - Use `Send test email` to verify delivery.

5. **Save the Rule:**
   - Click `Save notification rule`.

---
## 🔕 Manage Alert Suppression 

Suppress specific alerts (e.g., known tools, internal processes) to reduce alert fatigue and avoid noise from innocuous activities.

1. **Go to:**
   - `Defender portal > Settings > Microsoft Defender XDR > Rules > Alert tuning`

2. **Manage Rules:**
   - View the full list of alert tuning rules created in your org.
   - Use checkboxes to:
     - ✅ **Turn rule on**
     - ✏️ **Edit rule**
     - 🗑️ **Delete rule**

3. **Note on Editing Rules:**
   - While editing, you can choose to **release previously suppressed alerts**, even if they don’t match the updated rule conditions.

---
## Manage Indicators

Indicators of compromise (IoCs) allow security teams to define how the system detects, prevents, or allows specific entities such as files, IPs, URLs/domains, and certificates. Defender for Endpoint uses multiple engines to match and act upon these indicators:

### 🧠 Detection Engines

- **Cloud Detection Engine**: Continuously scans collected data for matches against defined IoCs.
    
- **Endpoint Prevention Engine (Defender AV)**: Honors IoC actions (e.g., block, allow).
    
- **Automated Investigation & Remediation (AIR)**: Ignores or blocks based on IoC verdicts.

### ✅ Supported IoC Actions

- **Allow**
    
- **Alert Only**
    
- **Alert and Block**

## 🔐 Indicator Types

### 1. Files

- Block malicious PE files (.exe, .dll)
    
- Prerequisites:
    
    - Windows Defender AV with cloud protection enabled
        
    - Windows 10 v1703+, Server 2016/2019
        
    - Antimalware client version 4.18.1901.x+
        
- Two methods:
    
    - Via Settings > Endpoints > Indicators
        
    - From file details page (contextual indicator)
        

> Note: Trusted signed files may behave differently; enforcement typically takes up to 30 minutes.

### 2. IPs and URLs/Domains

- Requires Network Protection in block mode
    
- Only supports **external IPs** and **single addresses** (no CIDR)
    
- Encrypted URLs can only be blocked in first-party browsers
    
- Latency: up to 2 hours
    

> Prerequisites:
> 
> - Windows 10 v1709+, Defender AV 4.18.1906.x+
>     
> - Custom Network Indicators enabled in Settings > Advanced features
>     

### 3. Certificates

- Allow/block behaviors for signed apps
    
- Supports .CER and .PEM formats
    
- Only leaf certificates are supported
    
- Cannot block Microsoft-signed certificates
    

> Prerequisites:
> 
> - Defender AV + cloud protection
>     
> - Windows 10 v1703+, Server 2016/2019
>     
> - Valid leaf cert or custom cert with trusted root
>     

---
## 📤 Import Indicators via CSV

You can bulk import IoCs using a CSV file:

### Supported Parameters

|Parameter|Type|Description|
|---|---|---|
|indicatorType|Enum|FileSha1, FileSha256, IpAddress, DomainName, Url|
|indicatorValue|String|Value of the indicator|
|action|Enum|Alert, AlertAndBlock, Allowed|
|title|String|Title of the alert|
|description|String|Description|
|expirationTime|DateTime|Format: YYYY-MM-DDTHH:MM:SS.0Z|
|severity|Enum|Informational, Low, Medium, High|
|recommendedActions|String|Suggested responder actions|
|rbacGroupNames|String|Comma-separated RBAC groups|
|category|String|Example: Execution, CredentialAccess|
|MITRE techniques|String|MITRE codes (comma-separated)|

---

## ⚙️ Managing Indicators

1. Go to **Settings > Endpoints > Indicators**.
    
2. Select entity tab (File, IP, URL/Domain, Certificate).
    
3. Add or edit indicators.
    
4. Define scope (RBAC groups, expiration, action).
    
5. Review and Save.
    

> Use indicators to enforce custom threat intelligence and block malicious activity with precision.