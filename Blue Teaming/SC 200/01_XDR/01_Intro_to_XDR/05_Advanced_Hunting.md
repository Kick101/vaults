Advanced hunting is a query-based threat-hunting tool that lets you explore up to 30 days of raw data. You can proactively inspect events in your network to locate threat indicators and entities. The flexible access to data enables unconstrained hunting for both known and potential threats.

### 🔍 Types of Data in Advanced Hunting

| **Data Type**             | **Description**                                             | **Update Frequency**                                                  | **Example Tables**                    |
| ------------------------- | ----------------------------------------------------------- | --------------------------------------------------------------------- | ------------------------------------- |
| **Event / Activity Data** | Real-time events: alerts, system activity, security events. | Near real-time (from sensors)                                         | `DeviceEvents`, `AlertEvidence`, etc. |
| **Entity Data**           | Info about users, devices, accounts (from AD, logs).        | Partial updates every **15 min**. Full consolidation every **24 hrs** | `DeviceInfo`, `IdentityInfo`          |

## 🛠️ Steps to Create a Detection Rule

### 1. **Prepare the Query**

- Use **Advanced Hunting** to create or select a query.
- Ensure the query returns:
    - `Timestamp`
    - `DeviceId`
    - `ReportId`
- Limit excessive alerts by **tuning your query**.

**📌 Example:**

```sql
DeviceEvents
| where Timestamp > ago(7d)
| where ActionType == "AntivirusDetection"
| summarize (Timestamp, ReportId) = arg_max(Timestamp, ReportId), count() by DeviceId
| where count_ > 5
```

### 2. Define Detection Rule Details

- **Name**
- **Alert Title**
- **Severity** (Informational, Low, Medium, High)
- **Category** (e.g., Malware, CredentialAccess)
- **MITRE ATT&CK** mapping (if applicable)
- **Description**
- **Recommended Actions**

### 3. Set Rule Frequency

|**Frequency**|**Lookback Period**|
|---|---|
|Hourly|Past 4 hours|
|Every 3 hours|Past 12 hours|
|Every 12 hours|Past 48 hours|
|Every 24 hours|Past 30 days|
|**Continuous**|Real-time (NRT)|

### 4. Choose Impacted Entities

Identify **one column** each for:

- Device (`DeviceId`)
- User (`AccountName`, `UserPrincipalName`)

### 5. Specify Automated Actions

📍 **Device Actions**:

- Isolate device
- Collect investigation package
- Run antivirus scan
- Initiate investigation

📍 **File Actions**:

- Block/Allow file (via SHA1 hash)
- Quarantine file

### 6. **Set Scope**

Apply to:
- All devices
- Specific device groups

### 7. **Review & Create**

- Confirm settings
- Click **Create**
- Rule runs immediately, then on the configured schedule

---
## Data schema

| Table name                               | Description                                                                                                                                                                                                 |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AlertEvidence                            | Files, IP addresses, URLs, users, or devices associated with alerts                                                                                                                                         |
| AlertInfo                                | Alerts from Microsoft Defender for Endpoint, Microsoft Defender for Office 365, Microsoft Cloud App Security, and Microsoft Defender for Identity, including severity information and threat categorization |
| CloudAppEvents                           | Events involving accounts and objects in Office 365 and other cloud apps and services                                                                                                                       |
| DeviceEvents                             | Multiple event types, including events triggered by security controls such as Windows Defender Antivirus and exploit protection                                                                             |
| DeviceFileCertificateInfo                | Certificate information of signed files obtained from certificate verification events on endpoints                                                                                                          |
| DeviceFileEvents                         | File creation, modification, and other file system events                                                                                                                                                   |
| DeviceImageLoadEvents                    | DLL loading events                                                                                                                                                                                          |
| DeviceInfo                               | Machine information, including OS information                                                                                                                                                               |
| DeviceLogonEvents                        | Sign-ins and other authentication events on devices                                                                                                                                                         |
| DeviceNetworkEvents                      | Network connection and related events                                                                                                                                                                       |
| DeviceNetworkInfo                        | Network properties of devices, including physical adapters, IP and MAC addresses, as well as connected networks and domains                                                                                 |
| DeviceProcessEvents                      | Process creation and related events                                                                                                                                                                         |
| DeviceRegistryEvents                     | Creation and modification of registry entries                                                                                                                                                               |
| DeviceTvmSecureConfigurationAssessment   | Threat & Vulnerability Management assessment events, indicating the status of various security configurations on devices                                                                                    |
| DeviceTvmSecureConfigurationAssessmentKB | Knowledge base of various security configurations used by Threat & Vulnerability Management to assess devices; includes mappings to various standards and benchmarks                                        |
| DeviceTvmSoftwareInventory               | Inventory of software installed on devices, including their version information and end-of-support status                                                                                                   |
| DeviceTvmSoftwareVulnerabilities         | Software vulnerabilities found on devices and the list of available security updates that address each vulnerability                                                                                        |
| DeviceTvmSoftwareVulnerabilitiesKB       | Knowledge base of publicly disclosed vulnerabilities, including whether exploit code is publicly available                                                                                                  |
| EmailAttachmentInfo                      | Information about files attached to emails                                                                                                                                                                  |
| EmailEvents                              | Microsoft 365 email events, including email delivery and blocking events                                                                                                                                    |
| EmailPostDeliveryEvents                  | Security events that occur post-delivery, after Microsoft 365 has delivered the emails to the recipient mailbox                                                                                             |
| EmailUrlInfo                             | Information about URLs on emails                                                                                                                                                                            |
| IdentityDirectoryEvents                  | Events involving an on-premises domain controller running Active Directory (AD). This table covers a range of identity-related events and system events on the domain controller.                           |
| IdentityInfo                             | Account information from various sources, including Microsoft Entra ID                                                                                                                                      |
| IdentityLogonEvents                      | Authentication events on Active Directory and Microsoft online services                                                                                                                                     |
| IdentityQueryEvents                      | Queries for Active Directory objects, such as users, groups, devices, and domains                                                                                                                           |
