### 🔔 **Security Alerts**

- Triggered by **advanced detections**
- Prioritized with relevant investigation and remediation info

### 📦 **Security Incidents**

- Grouped **collections of related alerts**
- Correlated using **Cloud Smart Alert Correlation**
- Provide a **single view** of an attack campaign

### 🧠 **Advanced Security Analytics**

1. **Integrated Threat Intelligence**
    - Sources: Microsoft 365, Azure, Outlook, DCU, MSRC, and third parties
    - Tracks **known bad actors and behaviors**
        
2. **Behavioral Analytics**
    - Matches to **known suspicious behavior patterns**
    - Detects **deviations from normal patterns**
    - Uses **ML and expert-driven models** to analyze logs
        
3. **Anomaly Detection**
	- Flags **anything that deviates from the baseline**    
    - Builds **baselines specific to your environment**
    - Flags **outlier behavior** indicating possible compromise

### **Continuous Monitoring and Assessments**

1. **Threat Intelligence** – Tracks new and known threats from many sources.
    
2. **Signal Sharing** – Shares security data across Microsoft services.
    
3. **Expert Teams** – Uses Microsoft’s in-house security specialists.
    
4. **Detection Tuning** – Improves alerts using real-world testing and feedback.

---
### **Accessing and Investigating Security Alerts**

- Go to **Defender for Cloud** > **Security Alerts**.
    
- Select an alert to open its side pane.
    
- Select **View full details** for comprehensive information.
    

**Left Pane Details:**

- Alert title, severity, status, activity time, and description.
    
- Affected resources and relevant Azure tags.
    

**Right Pane Tabs:**

- **Alert details**: Includes IP addresses, files, processes, etc.
    
- **Take action**:
    
    - _Mitigate the threat_: Manual remediation steps.
        
    - _Prevent future attacks_: Security recommendations.
        
    - _Trigger automated response_: Link a Logic App.
        
    - _Suppress similar alerts_: Ignore irrelevant repeated alerts.
        
![[Pasted image 20250731112057.png]]
### **Creating a Workflow Automation**

1. Go to **Defender for Cloud** > **Workflow automation**.
    
2. Click **Add workflow automation**.
    
3. Configure:
    
    - Name and description.
        
    - Trigger conditions (e.g., alerts containing “SQL”).
        
    - Associated Logic App.
        
4. Click **Create a new one** in Actions to build a Logic App.

### **Creating a Logic App**

- Fill in **Name**, **Resource Group**, **Location**, then select **Create**.
    
- Use predefined templates or build a custom flow.
    
- Supported triggers:
    
    - **When a Defender for Cloud Recommendation is created or triggered**
        
    - **When a Defender for Cloud Alert is created or triggered** (with severity filtering)

![[Pasted image 20250731112710.png]]

### **Manual Triggering**

- Open any security alert or recommendation.
    
- Click **Trigger Logic App** to execute manually.

---
## Suppress alerts from Defender for Cloud

- Automatically dismiss alerts that:
    
    - Are false positives.
        
    - Occur too frequently to be actionable.
        

> *Note*: Suppression rules only affect alerts already triggered on selected subscriptions.

### **Create a Suppression Rule**

**From the Security Alerts page in Defender for Cloud:**

- Option 1: Click the **ellipsis (…)** next to an alert > **Create suppression rule**.
    
- Option 2: Click **Suppression rules** at the top > **Create new suppression rule**.

### **Configure Suppression Rule**

**In the new suppression rule pane:**

- **Scope**:
    
    - Apply rule to all similar alerts.
        
    - Or apply based on specific criteria (IP address, process name, user, Azure resource, or location).
        
- **Rule Details**:
    
    - **Name**: Must start with a letter/number, be 2–50 characters, use only dashes or underscores.
        
    - **State**: Enabled or Disabled.
        
    - **Reason**: Choose from built-in options or "Other".
        
    - **Expiration**: Up to 6 months.
        
- **Optional**:
    
    - Use **Simulate** to preview which alerts would have been dismissed.
        
- Click **Save** to create the rule.

![[Pasted image 20250731125221.png]]
### **View Suppressed Alerts**

- Suppressed alerts are still generated, but their **state** is set to **Dismissed**.
    
- To view them:
    
    - Go to **Security Alerts** > open **Filters** > select **Dismissed**.

---
## **Generate Threat Intelligence Reports**

- Defender for Cloud connects data from **multiple alerts** to reveal **attack campaigns**.
    
- Helps analysts **triage**, **investigate**, and **remediate** threats faster.
    
- Provides a **single view** of correlated incidents, attacker behavior, and impacted resources.
### **View Security Incidents**

1. Go to **Defender for Cloud > Security alerts tile**.
    
2. Incidents are displayed with a **distinct icon** from regular alerts.
    
3. Select an incident to open the **Security Incident page**:
    
    - **Left pane**: title, severity, status, time, description, affected resources, and Azure tags.
        
    - **Right pane**:
        
        - **Alerts tab**: shows alerts involved in the incident.
            
        - **Take action tab**:
            
            - Mitigate the threat (manual remediation steps)
                
            - Prevent future attacks (security posture improvements)
                
            - Trigger automated response (Logic App)
                
            - Suppress similar alerts (if deemed non-relevant)
                

> Follow remediation guidance under each alert to mitigate the full incident.

### **Threat Intelligence Reports Overview**

- Generated when Defender for Cloud identifies a threat.
    
- Contain detailed insight on:
    
    - **Attacker identity/associations** (if available)
        
    - **Objectives** and **tactics, techniques, and procedures (TTPs)**
        
    - **Indicators of Compromise (IoCs)** – URLs, file hashes
        
    - **Campaign history** and activity patterns
        
    - **Victimology** – industry/geographic prevalence
        
    - **Mitigation & remediation steps**
        

### **Types of Reports**

1. **Activity Group Report**: Deep dive into attacker profile and TTPs.
    
2. **Campaign Report**: Focuses on details of specific attack campaigns.
    
3. **Threat Summary Report**: Comprehensive coverage including both of the above.
    

### **How to Access a Threat Intelligence Report**

1. Navigate to **Defender for Cloud > Security alerts page**.
    
2. Select an alert to open its **details page**.
    
3. Click the **threat intelligence report link** under the alert description.
    
4. The report opens as a **PDF** in your browser.

---

##  **Defender for Key Vault Alerts**

### **Contact & Verification**

- Confirm if the alert source (Object ID, UPN, or IP) belongs to your tenant.
    
    - If **firewall is enabled**, check if access was intentionally granted.
        
    - If **source is legitimate**, contact the user/app owner to verify.
        

### **Immediate Mitigation**

- **Unknown IP Address**:
    
    - Enable Key Vault firewall.
        
    - Limit access to **trusted IPs and VNets**.
        
- **Suspicious App/User**:
    
    - Remove or restrict access via **Key Vault Access Policies**.
        
- **Microsoft Entra Role**:
    
    - Contact a Global Admin.
        
    - Reassess and **revoke or limit roles** if unnecessary.
        

### **Impact Analysis**

- Go to **Key Vault > Security > Alerts**.
    
- Identify accessed secrets, keys, or certificates.
    
- Use **diagnostic logs** to trace historical activity.
    

### **Remediation**

- Immediately **rotate** affected secrets, keys, and certificates.
    
- **Disable or delete** compromised secrets.
    
- Alert app owners to:
    
    - Audit usage.
        
    - Identify information accessed.
        
    - Contain and mitigate impact.

---
##  Defender for DNS Alerts

### **Contact & Verification**

- Contact the resource owner to validate activity.
    
    - **If expected**, dismiss the alert.
        
    - **If unexpected**, treat the resource as compromised.
        

### **Immediate Mitigation**

- **Isolate** the machine from the network.
    
- Run a **full antimalware scan** and follow remediation steps.
    
- Remove **suspicious software/packages**.
    
- **Reimage or restore** from a known-good source if necessary.
    
- Address all **Defender for Cloud security recommendations** to harden the system.

---
##  Defender for Resource Manager Alerts

### **Contact & Verification**

- Contact the relevant user/resource owner.
    
    - If **expected**, dismiss.
        
    - If **unexpected**, assume compromise.
        

### **Immediate Mitigation**

#### ✅ **User Accounts**

- **Unfamiliar**: Delete immediately.
    
- **Familiar**: Reset credentials.
    
- Use **Activity Logs** to trace suspicious operations.
    

#### ✅ **Subscriptions**

- Delete unknown **Runbooks** in Automation Accounts.
    
- Audit and **revoke IAM roles** of suspicious accounts.
    
- Delete **unfamiliar resources**.
    
- Review and address **related security alerts**.
    
- Use **Activity Logs** for deep investigation.
    

#### ✅ **Virtual Machines**

- Change all user passwords.
    
- Perform a **full malware scan**.
    
- **Reimage** using a trusted, clean base image.