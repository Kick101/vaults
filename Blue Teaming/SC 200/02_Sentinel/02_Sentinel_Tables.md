Sentinel has Analytic Rules that generate Alerts & Incidents based on querying the tables within Log Analytics.

| Table                         | Description                                                                                                                                 |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `SecurityAlert`               | Contains Alerts Generated from Sentinel *Analytical Rules*. Also, it could include Alerts created directly from a Sentinel *Data Connector* |
| `SecurityIncident`            | Alerts can generate Incidents. Incidents are related to Alert(s).                                                                           |
| `ThreatIntelligenceIndicator` | Contains user-created or data connector ingested Indicators such as File Hashes, IP Addresses, Domains                                      |
| `Watchlist`                   | A Microsoft Sentinel watchlist contains imported data.                                                                                      |

---
When Sentinel ingests data from the Data Connectors, the following table lists the most commonly used tables.

| Table                   | Description                                                                                                                                               |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `AzureActivity`         | Entries from the Azure Activity log that provides insight into any subscription-level or management group level events that occurred in Azure.            |
| `AzureDiagnostics`      | Stores resource logs for Azure services that use Azure Diagnostics mode. *Resource logs* describe the internal operation of Azure resources.              |
| `AuditLogs`             | Audit log for Microsoft *Entra ID*. Includes system activity information about user and group management, managed applications, and directory activities. |
| `CommonSecurityLog`     | Syslog messages using the Common Event Format (CEF).                                                                                                      |
| `McasShadowItReporting` | Microsoft Defender for *Cloud Apps logs*                                                                                                                  |
| `OfficeActivity`        | Audit logs for *Office 365* tenants collected by Microsoft Sentinel. Including Exchange, SharePoint and Teams logs.                                       |
| `SecurityEvent`         | Security *events collected from windows machines* by Azure Security Center or Microsoft Sentinel                                                          |
| `SigninLogs`            | Azure *Activity Directory Sign-in* logs                                                                                                                   |
| `Syslog`                | Syslog events on *Linux computers* using the Log Analytics agent.                                                                                         |
| `Event`                 | *Sysmon* Events collected from a Windows host.                                                                                                            |
| `WindowsFirewall`       | Windows Firewall Events                                                                                                                                   |

---
## Defender XDR tables

| Table name                  | Description                                                                                                                                                                       |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `AlertEvidence`             | Files, IP addresses, URLs, users, or devices associated with alerts                                                                                                               |
| `CloudAppEvents`            | Events involving accounts and objects in Office 365 and other cloud apps and services                                                                                             |
| `DeviceEvents`              | Multiple event types, including events triggered by security controls such as Windows Defender Antivirus and exploit protection                                                   |
| `DeviceFileCertificateInfo` | Certificate information of signed files obtained from certificate verification events on endpoints                                                                                |
| `DeviceFileEvents`          | File creation, modification, and other file system events                                                                                                                         |
| `DeviceImageLoadEvents`     | *DLL* loading events                                                                                                                                                              |
| `DeviceInfo`                | Machine and OS information                                                                                                                                                        |
| `DeviceLogonEvents`         | Sign-ins and other authentication events on devices                                                                                                                               |
| `DeviceNetworkEvents`       | Network connection and related events                                                                                                                                             |
| `DeviceNetworkInfo`         | Network properties of devices, including physical adapters, *IP and MAC* addresses, as well as connected networks and domains                                                     |
| `DeviceProcessEvents`       | Process creation and related events                                                                                                                                               |
| `DeviceRegistryEvents`      | Creation and modification of registry entries                                                                                                                                     |
| `EmailEvents`               | Microsoft 365 email events, including email delivery and blocking events                                                                                                          |
| `EmailPostDeliveryEvents`   | Security events that occur post-delivery, after Microsoft 365 has delivered the emails to the recipient mailbox                                                                   |
| `EmailUrlInfo`              | Information about URLs on emails                                                                                                                                                  |
| `EmailAttachmentInfo`       | Information about files attached to Office 365 emails                                                                                                                             |
| `IdentityDirectoryEvents`   | Events involving an on-premises domain controller running Active Directory (AD). This table covers a range of identity-related events and system events on the domain controller. |
| `IdentityLogonEvents`       | Authentication events on Active Directory and Microsoft online services                                                                                                           |
| `IdentityQueryEvents`       | Queries for Active Directory objects, such as users, groups, devices, and domains                                                                                                 |

---

