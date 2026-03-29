- [[#Safe Attachments Policy|Safe Attachments Policy]]
	- [[#Safe Attachments Policy#🦠 Malware scanning option for email attachments|🦠 Malware scanning option for email attachments]]
	- [[#Safe Attachments Policy#🔁 Redirect Attachment on Detection|🔁 Redirect Attachment on Detection]]
- [[#Safe Links Policy|Safe Links Policy]]
	- [[#Safe Links Policy#Safe Links policy configuration|Safe Links policy configuration]]
- [[#🔐 Anti-Phishing Policy|🔐 Anti-Phishing Policy]]
	- [[#🔐 Anti-Phishing Policy#⚠️ No Default anti-phishing Policy|⚠️ No Default anti-phishing Policy]]
	- [[#🔐 Anti-Phishing Policy#🕵️‍♂️ Impersonation Detection|🕵️‍♂️ Impersonation Detection]]
	- [[#🔐 Anti-Phishing Policy#🛠️ Configuration|🛠️ Configuration]]


---
## Safe Attachments Policy

1. All messages and attachments that don't have a known virus/malware signature are routed to a special environment 
2. where MDO uses various *machine learning and analysis techniques* to detect malicious intent. 
3. If no suspicious activity is detected, the message is released for delivery to the mailbox.

![[Pasted image 20250702191748.png]]

### 1. 🦠Malware scanning option for email attachments

| **Option**           | **Description**                                                                                                                                  | **Risk Level**      | **Recommended For**                               |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------- | ------------------------------------------------- |
| **Off**              | No scanning is performed. Attachments are **delivered as-is**, regardless of malware.                                                            | 🔴 High Risk        | Never use in production environments              |
| **Monitor**          | **Allows** delivery of the message and attachment **even if malware is detected**. Logs and tracks the result, but **takes no blocking action**. | 🟠 Medium–High Risk | Testing, policy tuning, threat hunting            |
| **Block**            | **Blocks** the entire message if malware is detected in the attachment. Current and **future instances** of the same malware are also blocked.   | 🟢 Low Risk         | Standard for secure production environments       |
| **Replace**          | **Strips the malicious attachment**, but **delivers the rest of the message** (e.g., subject, body).                                             | 🟡 Moderate Risk    | When delivery is important but file access is not |
| **Dynamic Delivery** | **Delivers message body immediately**, **holds the attachments for scanning**, and **reattaches** them **only if safe**.                         | 🟢 Low Risk         | Balanced option for speed and security            |


### 2. 🔁Redirect Attachment on Detection

This feature forwards attachments (that were *blocked*, *replaced*, or *monitored*) to a **specific email address** — usually a **security administrator**.

1. **Enable redirect**
2. **Apply if malware scanning times out or errors**
    - ✅ When checked, the same redirect behavior applies **even if scanning fails or times out** (e.g., due to large files or service errors).
    - 📉 This prevents losing visibility on **attachments that weren’t fully scanned**, giving security teams a chance to analyze what might have slipped through.

>You can create a transport rule, also known as a `mail flow rule`, in the **Exchange admin center** (EAC) to bypass safe attachments scanning. 
>- Modify the message properties to set a message header: `X-MS-Exchange-Organization-SkipSafeAttachmentProcessing` 

---
## Safe Links Policy

**Safe Links supported apps:**

- Microsoft 365 apps for enterprise on Windows or Mac
- Office for the web (Word for the web, Excel for the web, PowerPoint for the web, and OneNote for the web)
- Word, Excel, PowerPoint, and Visio on Windows, as well as Office apps on iOS and Android devices
- Microsoft Teams channels and chats

 Additionally, Safe links can be configured to support links in **Office 2016** clients where the user is signed in with their Office 365 credential.

>**Safe links include a default global settings policy:**
>- You can't delete this policy.
>- It's recommended that you apply Microsoft Defender for Office 365 safe links policies to ALL users in your organization.

### Safe Links policy configuration

![[Pasted image 20250703114340.png |]]

- **Scan unknown URLs in emails**   
    ✅ _On:_ Rewrites and checks unknown URLs for safety.
- **Scan downloadable content in URLs** 
    ✅ _On:_ Opens linked files (like PDFs) in a secure environment. If malicious, shows users a warning.
- **Protect internal emails**   
    ✅ _On:_ Safe Links applies to emails sent **within your organization**, not just external.
- **Do not track clicked links**   
    ❌ _Unchecked:_ Microsoft tracks which Safe Links are clicked (recommended, default).  
    ✅ _Checked:_ Disables tracking.
- **Block access to malicious URLs**   
    ✅ _On:_ Prevents users from bypassing the warning and visiting dangerous sites.
- **Exclude known safe URLs from rewriting**   
    ➕ Add trusted URLs (like partner websites) here to **skip Safe Link scanning**

>Similar to bypassing safe attachments, you can also create a transport rule to bypass safe links. The message header for bypassing safe links is `X-MS-Exchange-Organization-SkipSafeLinksProcessing`
>
>MDO will **not honor headers added by external/unauthenticated sources**.

---
## 🔐 Anti-Phishing Policy

![[Pasted image 20250703115142.png ]]

- Uses **machine learning models** to analyze **incoming emails** for phishing indicators.
- Evaluates emails when users are covered by:
    - **Safe Attachments**
    - **Safe Links**
    - **Anti-phishing policies**

### ⚠️ No Default anti-phishing Policy

- **Admins must configure** anti-phishing policies.
- Initially, **only target selection** (users/groups/domains) is required.

### 🕵️‍♂️ Impersonation Detection

Detects subtle spoofing of:

- **Domains**: 
    E.g., `contoso.com` → `ćóntoso.com`    
- **Users**:  
    E.g., `michelle@contoso.com` → `michele@contoso.com`

Even when such domains appear **technically legitimate** (with valid `DNS, SPF/DKIM/DMARC`), but intent is to deceive recipients.

### 🛠️ Configuration

- A set of users and domain to protect
- **Actions** (redirect or sending to junk, quarantine, etc.)
- **Safety tips** for suspicious emails
- **Trusted senders/domains** (to reduce false positives)
- **Anti-spoofing** logic

>Anti-spoofing settings are also included in MDO anti-phishing policies.







