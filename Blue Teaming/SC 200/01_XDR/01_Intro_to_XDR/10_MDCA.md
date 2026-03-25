**Cloud access security broker** are the intermediaries between users and all of the cloud services they access. CASBs help you to apply monitoring and security controls over your users and data. CASBs for cloud services are *like firewalls* to corporate networks.

## ☁️ MDCA Framework: Four Pillars

### 1. Discover and Control Shadow IT
- **Goal**: Identify and control the use of unsanctioned apps (Shadow IT).
- **How**:
  - Discover cloud apps, IaaS, and PaaS services in use.
  - Analyze app usage patterns to assess risk.
- **Action**: Block or govern unsanctioned apps.

### 2. Protect Sensitive Information in the Cloud
- **Goal**: Prevent accidental or unauthorized data exposure.
- **How**:
  - Use **Data Loss Prevention (*DLP*)** policies.
  - Classify and protect data at rest across services.
- **Action**: Enforce policies to restrict sharing or downloading of sensitive content.

### 3. Protect Against Cyberthreats and Anomalies
- **Goal**: Detect abnormal behavior and possible threats.
- **How**:
  - Use **User and Entity Behavior Analytics (*UEBA*)**.
  - Detect anomalies like ransomware or impossible travel.
- **Action**: Trigger alerts or automatic responses.

### 4. Assess Cloud App Compliance
- **Goal**: Ensure cloud apps align with **regulatory standards** (*GDPR, ISO 27001*).
- **How**:
  - Compliance scoring
  - Risk assessment dashboards
- **Action**: Block or limit usage of non-compliant apps.

---
## 📊 Cloud Discovery Dashboard

>Cloud Discovery analyzes your traffic logs against a catalog of *more than 16,000 cloud apps*.

Cloud Discovery ranks each app and scores it based on *more than 80 risk factors* to give you visibility into cloud use, Shadow IT, and the risk it poses in your organization.

### 1. High-Level Usage Overview
- Get an overview of cloud app usage.
- Identify top users and source IPs.
- Focus on high-usage users for further monitoring.

### 2. App Categories
- Check which app categories are most used.
- Look at how much usage comes from **Sanctioned apps**.

### 3. Discovered Apps Tab
- View all apps in each category.
- Dive deeper into specific app usage.

### 4. App Risk Overview
- Review risk scores (1 to 10) based on:
  - Security
  - Compliance
  - Regulatory measures

### 5. App Headquarters Map
- See app origin based on company HQ location.

### 6. Flagging Risky Apps
- Mark risky apps as **Unsanctioned** in the Discovered apps view.

>*MDE* (or a similar solution), any unsanctioned app is automatically blocked.

>If you don't have a threat protection solution, you can run a script against the data source to block the app. Then users will see a notification that the application is blocked when they try to access it.

---
## Classify and Protect Sensitive Information

Information protection helps *prevent accidental or malicious exposure of sensitive data*. Microsoft Defender for Cloud Apps integrates with *Azure Information Protection* (AIP) to classify, label, and protect information.

>Requires enabling the Microsoft 365 app connector.

### Phase 1: Discover Data
- Connect apps to Defender for Cloud Apps.
- Use:
  - **App connectors** (e.g., for SharePoint, OneDrive).
  - **Conditional Access App Control** for real-time access.

### Phase 2: Classify Sensitive Information
- Define what is sensitive for the organization.
- Use built-in or custom labels from AIP.

##### Default AIP (Azure Information Protection) Labels:
| Label               | Description                                 |
| ------------------- | ------------------------------------------- |
| Personal            | Non-business use only.                      |
| Public              | Publicly shareable (e.g., marketing).       |
| General             | Internal or partner use.                    |
| Confidential        | Could damage org if leaked.                 |
| Highly Confidential | Serious damage if leaked (e.g., passwords). |

- Enable AIP integration: `Settings > Automatically scan new files for AIP classification labels`

### Phase 3: Protect Data (File Policies)
- Policies scan real-time and at-rest content.
- Actions:
  - Alert/Email notification
  - Change sharing access
  - Quarantine files
  - Remove permissions
  - Move to trash

### Phase 4: Monitor and Report
- Use **Alerts pane** and filter by `Category: DLP`
- Investigate or dismiss alerts
- Export alerts to `.csv` for further analysis

---
# Threat Detection

Detecting cyberthreats and anomalies is a core part of the Microsoft Defender for Cloud Apps framework. It uses machine learning and user/entity behavior analytics (UEBA) to detect threats based on deviations from normal activity patterns.

### Learning Phase
- **7-day learning period** to establish behavioral baselines.
- Learns from IPs, devices, locations, services used, and risk scores.
- Reduces false positives through pattern recognition.

### Risk Evaluation Factors (30+ Indicators grouped):
- Risky IP address
- Login failures
- Admin activity
- Inactive accounts
- Location
- Impossible travel
- Device and user agent
- Activity rate

### Anomaly Detection Policies
Policies are automatically enabled and monitor user sessions across cloud apps.

Popular detections:
- **Impossible travel**
- **Activity from infrequent country**
- **Malware detection**
- **Ransomware activity**
- **Suspicious IP activity**
- **Suspicious inbox forwarding**
- **Unusual multiple file downloads**
- **Unusual admin activity**

### Configure Discovery Anomaly Policy
- Monitors spikes in cloud app usage: downloads, uploads, transactions, and user count.
- Compares metrics against baselines.
- Customize with:
  - App filters
  - Data views
  - Start date
  - Sensitivity level

### Fine-tuning and Suppression
- **Goal:** Reduce false positives and alert fatigue.
- **Sensitivity Levels:**
  - Low: Suppresses System, Tenant, and User
  - Medium: Suppresses System and User
  - High: Suppresses System only

#### Suppression Types:
| Type    | Description |
|---------|-------------|
| System  | Always suppressed built-in detections |
| Tenant  | Suppress based on org-wide activity |
| User    | Suppress based on individual history |

### Adjusting Policy Scope
Each policy can target specific users/groups.

**Steps:**
1. Go to Microsoft Defender Portal
2. Navigate to **Cloud apps > Policies > Policy management**
3. Filter by **Anomaly detection policy**
4. Select and edit the desired policy
5. Under **Scope**, choose **Specific users and groups**
6. Configure **Include** and **Exclude** settings
7. Select **Update** to save changes




