AIR in Microsoft Defender for Office 365 includes certain remediation actions. Whenever an automated investigation is running or has completed, you'll typically see one or more remediation actions that require approval by your security operations team to proceed. Such remediation actions include:

- Soft delete email messages or clusters
- Block URL (time-of-click)
- Turn off external mail forwarding
- Turn off delegation

---
## 🚀 How an automated investigation starts

| **Step**                           | **Description**                                                                                                                                                   |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Alert Triggered**             | A threat detection (e.g., malicious file, behavior anomaly) generates an alert.                                                                                   |
| **2. Security Playbook Activated** | A predefined playbook tied to that alert type automatically executes.                                                                                             |
| **3. Investigation Begins**        | If the playbook supports automation, Defender for Endpoint launches an **automated investigation**.                                                               |
| **4. Scope Expansion**             | Defender scans the organization for the same malicious indicator (e.g., file hash, behavior). If found on other devices, those are included in the investigation. |
| **5. Verdict Assignment**          | Entities involved are categorized as: **Malicious**, **Suspicious**, or **No threats found**.                                                                     |
| **6. Results Accessible**          | Investigation progress and outcomes are available **in real-time** and post-analysis via the **Investigation dashboard**.                                         |

## Details of an automated investigation

| **Tab**             | **Purpose / Description**                                                                                             |
| ------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Alerts**          | Displays the triggering alert(s) that initiated the investigation.                                                    |
| **Devices**         | Lists the affected endpoint(s) where malicious activity was detected.                                                 |
| **Evidence**        | Highlights malicious artifacts identified (e.g., files, scripts, registry entries).                                   |
| **Entities**        | Shows all analyzed items with verdicts: **Malicious**, **Suspicious**, or **No Threats Found**.                       |
| **Log**             | A **chronological audit trail** of every action the automated investigation performed.                                |
| **Pending Actions** | Appears only when human approval is needed. Analysts can **Approve** or **Reject** these automated remediation steps. |

---
## 🔄 How Automated Investigation Expands Scope

| **Trigger**                      | **Scope Expansion Behavior**                                                                                                                                                                           |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **New Alerts on Same Device**    | If additional alerts are generated on the same device during an ongoing investigation, they are automatically included in that investigation.                                                          |
| **Threat Seen on Other Devices** | If the same threat indicator (e.g., malicious hash, IP, domain) is detected on **other devices**, those devices are **automatically added** to the investigation.                                      |
| **Entity Reappearance**          | If an **incriminated entity** (e.g., file, user, script) appears on a different device, a **new automated playbook** runs on that device.                                                              |
| **>10 Devices Affected**         | If more than **10 devices** are pulled into the investigation due to entity spread, this triggers a **Pending Action** requiring **manual analyst approval** (visible in the **Pending Actions tab**). |

---
## 🛠️ How Threats Are Remediated

| **Phase**                         | **Details**                                                                                                                                                                                                               |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Evidence Verdicts**          | As part of an automated investigation, each entity (file, process, registry key, etc.) is analyzed and assigned a **verdict**: • **Malicious** • **Suspicious** • **No threats found**                                    |
| **2. Remediation Triggered**      | If the verdict is **Malicious** or **Suspicious**, the system may initiate remediation actions.                                                                                                                           |
| **3. Common Remediation Actions** | Examples include: • **Quarantining malicious files** • **Stopping harmful processes or services** • **Removing persistence mechanisms** (e.g., scheduled tasks, registry entries)                                         |
| **4. Automation Level Dependent** | Actions can be: • **Fully automated** (no analyst interaction) • **Approval-based** (SOC team must authorize) This behavior depends on your **automation level**, **security settings**, and **PUA protection policies**. |
| **5. Centralized Oversight**      | All actions—**pending or completed**—are tracked in the **[Action Center](https://security.microsoft.com)**. Analysts can review, approve, or **undo** remediation actions if needed.                                     |

---
## ⚙️ Automation Levels in Automated Investigation and Remediation (AIR)

| **Level**                                  | **Behavior**                                                                                                | **Folder Sensitivity**           | **Review Location**                                          |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------------------- | -------------------------------- | ------------------------------------------------------------ |
| **Full Automation**                        | ✅ Automatically remediates all malicious artifacts.                                                         | No restrictions.                 | All actions recorded in **Action Center → History**.         |
| **Semi – All Remediation Needs Approval**  | 🕒 No action is taken without analyst approval.                                                             | All folders.                     | Pending items appear in **Action Center → Pending**.         |
| **Semi – Core Folder Approval Needed**     | ✅ Auto-remediation for non-core folders. 🕒 Core system directories (e.g., `C:\Windows\`) require approval. | System (core) folders protected. | Core folder actions → **Pending** tab; others → **History**. |
| **Semi – Non-Temp Folder Approval Needed** | ✅ Auto-remediation only in temporary folders. 🕒 All other folders require approval.                        | Non-temp folders protected.      | Temp folder actions → **History**; others → **Pending**.     |

> #### 📌 Notes:
> - All actions—**pending or completed**—are visible in the **Action Center**.
> - **Undo capability** is available for any completed remediation.
> - **Full automation** is **recommended** for faster, organization-wide threat response.


**Temporary folders can include the following examples:**

- `\users*\appdata\local\temp*`
- `\documents and settings*\local settings\temp*`
- `\documents and settings*\local settings\temporary*`
- `\windows\temp*`
- `\users*\downloads*`
- `\program files\`
- `\program files (x86)*`
- `\documents and settings*\users*`
