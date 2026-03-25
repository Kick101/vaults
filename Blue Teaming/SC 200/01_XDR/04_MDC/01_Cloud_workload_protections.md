## 🔧 Azure Arc

- Use **Azure Arc** to onboard and protect non-Azure machines as Azure resources.

Azure Arc connected systems have the Azure Connected Machine agent installed on them, and when they're added to a *Data Collection Rule* (DCR) the *Azure Monitor agent extension* is also installed on them. 

>When you enable Defender for Cloud, the security policy built-in to Defender for Cloud is reflected in Azure Policy as a built-in initiative under the Defender for Cloud category.

---
## 1. Defender for Servers 

**Plan 1** (*Baseline*):

- Deploys **Microsoft Defender for Endpoint** automatically to cloud workloads
    
- **Hourly billing** (cost-effective for ephemeral VMs)
    
- Integrates **alerts and vulnerability data** into Defender for Cloud
    

**Plan 2** (*Advanced*):

- Includes **Plan 1 features**
    
- Adds:
    
    - **Agentless scanning** for software inventory, secrets, malware
        
    - **Vulnerability assessments**
        
    - **Security posture recommendations**

### **Data Collection**

- **Windows:** Integrated with Azure services
    
- **Linux:** Uses **auditd** for audit logs
    
- **Hybrid/Multicloud:** Managed via **Azure Arc**

|**Feature**|**Plan 1**|**Plan 2**|**Notes**|
|---|---|---|---|
|**Multicloud & Hybrid Support**|✅|✅|Azure, AWS, GCP, on-prem via Azure Arc|
|**Defender for Endpoint (MDE) Integration**|✅|✅|Includes automatic onboarding, EDR, OS-level detection, integrated alerts|
|**Software Inventory & Agent-Based Vulnerability Scanning**|✅|✅|Uses Defender Vulnerability Management|
|**Agentless Features**|❌|✅|Vulnerability, malware, and secrets scanning|
|**Azure Network Threat Detection**|❌|✅|Agentless detection at control plane level|
|**OS Baseline Misconfiguration Detection**|❌|✅|Uses Azure Policy and MCSB benchmarks|
|**Regulatory Compliance**|✅|✅|Baseline included; more standards with paid plans|
|**OS Update Assessment**|❌|✅|Uses Azure Update Manager; Arc needed for non-Azure|
|**Defender Vulnerability Management (Premium)**|❌|✅|Includes cert assessments, baseline checks (Portal only)|
|**File Integrity Monitoring (FIM)**|❌|✅|MDE-based; legacy MMA deprecated|
|**Just-In-Time VM Access**|❌|✅|Reduces attack surface by restricting port access|
|**Network Map**|❌|✅|Geographic visualization of network security recommendations|
|**Log Analytics Free Data Ingestion**|❌|✅|500 MB/month for select data types|

---
## 2. Defender for App Service

Provides **threat detection** and **security monitoring** for web apps and APIs hosted in **Azure App Service**.

### **Key Features**

- **Analyzes cloud-scale telemetry** to detect:
    
    - **Probing, crawling, and distributed attacks**
        
    - **Attack patterns** across multiple App Service instances
        
- **Inspects and logs requests/responses** before reaching the app
    
- **Correlates attacks** that would be invisible from a single-host perspective

### **Protection Scope**
    
- App Service VM instances
	
- Management interfaces
	
- Traffic to/from your apps
        
- For **Windows-based plans**:
    
    - Accesses underlying sandboxes and VMs.
        
    - Can detect attacks **even after compromise** if Defender is enabled later

### **How to Enable Protection**

- Use a **supported App Service plan** on dedicated infrastructure.
    
- Enable **Microsoft Defender for Cloud** (can selectively enable App Service plan).
    
- **No deployment or onboarding needed**—integration is native and automatic.

----
## 3. Defender for Storage

- **Azure-native security layer** for detecting suspicious or malicious activity targeting Azure Storage accounts.
    
- Uses **AI and Microsoft Threat Intelligence** to:
    
    - Detect unusual access or exploitation attempts.
        
    - Trigger **contextual alerts** with investigation and remediation guidance.
        
- **Alerts** are:
    
    - Integrated into **Microsoft Defender for Cloud**.
        
    - Emailed to **subscription admins** with detailed information and recommendations.

- Supports **Blob Storage**, **Azure Files**, and **Data Lakes**.

- **Wide detection coverage**, including:

	- Anonymous access
	    
	- Compromised credentials
	    
	- Social engineering attacks
	    
	- Privilege misuse
	    
	- Malicious uploads

### **Alert Types**

- **Suspicious access**:
    
    - Access from **Tor nodes**
        
    - IPs flagged by Microsoft Threat Intelligence
        
- **Suspicious activities**:
    
    - Anomalous **data extraction**
        
    - Unexpected **permission changes**
        
- **Malicious uploads**:
    
    - Files with known **malware hashes**
        
    - Phishing content
        
- Alerts include:
    
    - Incident details
        
    - Investigation/remediation recommendations
        
    - Option to **export to Azure Sentinel or third-party SIEMs**

### **Hash Reputation Analysis (Malware Detection)**

- Instead of scanning files directly, Defender:
    
    - Analyzes **storage logs**
        
    - Compares **file hashes** to known malware (viruses, trojans, ransomware, etc.)

---
## 4. Defender for SQL

### **Plans**

#### 1. **Defender for Azure SQL Database Servers**

- Protects:
    
    - **Azure SQL Database**
        
    - **Azure SQL Managed Instance**
        
    - **Dedicated SQL pools in Azure Synapse**
        

#### 2. **Defender for SQL Servers on Machines**

- Extends protection to:
    
    - **SQL Server on VMs**
        
    - **On-prem SQL Servers**:
        
        - With or without Azure Arc (Arc support in preview)

### **Key Features**

- **Vulnerability Assessment**:
    
    - Scans and reports on SQL security posture.
        
    - Provides details and remediation guidance.
        
- **Advanced Threat Protection**:
    
    - Detects SQL injection, brute-force attacks, and privilege abuse.
        
    - Sends detailed alerts with investigation/remediation steps.

### **Types of Alerts (All SQL Types)**

- SQL Injection Attempts:        
- Anomalous Access/Query Patterns:
    - Example: *high failed login attempts* (brute-force indication)
- Suspicious Activity:
    - Example: legitimate user accessing from a breached system linked to a crypto-mining C&C server

**Brute-Force Attacks**

- Can differentiate:
    
    - Simple brute-force attempts
        
    - Brute force on valid accounts
        
    - Successful brute-force attacks

---
## 5. Defender for Key Vault

- Azure Key Vault stores **encryption keys**, **secrets**, **certificates**, **connection strings**, and **passwords**.
    
- Enabling Defender for Key Vault provides **Azure-native threat protection**, adding a layer of **security intelligence**.

### Key Benefits

- Detects **unusual or malicious attempts** to access or exploit Key Vault.
    
- No need for security expertise or third-party monitoring tools.
    
- Alerts include:
    
    - **Incident details**
        
    - **Investigation guidance**
        
    - **Remediation steps**
        
- Alerts can be **emailed** to relevant organizational members.
    
- All alerts are visible in:
    
    - **Key Vault's Security page**
        
    - **Microsoft Defender for Cloud dashboard**

### **Alert Handling**

- Always **investigate Key Vault alerts**, even if triggered by known applications or users.
    
- Defender protects **application secrets** and **credentials**—all alerts should be taken seriously.

---
## 6. Defender for Resource Manager

- **Azure Resource Manager (ARM)** is the deployment and management service for Azure.
- It provides a management layer to:

	- Create resources
	    
	- Update resources
	    
	- Delete resources
    
- Management features include:

	- Access control
	    
	- Locks
	    
	- Tags
    
- As the core **management layer**, it's tightly integrated with all resources and is a *high-value attack surface*.
    
- Automatically monitors resource management operations:
	- Azure portal
	- Azure REST APIs
	- Azure CLI
	- Other Azure programmatic clients
- Uses **advanced security analytics** to:
	- Detect threats
	- Alert on suspicious activity

> *Management Layer* Interfaces and services for controlling and monitoring resources.

### **How to Investigate Alerts**

Alerts are generated based on monitoring Azure Resource Manager operations using:

- Internal logs from Azure Resource Manager
    
- Azure Activity Log (provides subscription-level event data)

**Investigation Steps:**

1. Open **Azure Activity Log**.
    
2. Apply filters:
    
    - Subscription referenced in the alert
        
    - Timeframe when activity was detected
        
    - Related user account (if applicable)
        
3. Look for suspicious or unusual activities.

----
## 7. Defender for DNS

- **Monitoring** all DNS queries from Azure resources
    
- Running **advanced security analytics** to detect and alert on suspicious DNS activity
- Protects against:

- **DNS tunneling** used for data exfiltration
    
- **Malware** communicating with command-and-control (C&C) servers
    
- **Connections to malicious domains**, including:
    
    - Phishing sites
        
    - Crypto mining infrastructure
        
- **DNS attacks** involving malicious DNS resolvers
    

---
## 8. Defender for Containers

1. **Environment Hardening**
- Azure Policy for Kubernetes:
    
    - Monitors API server requests against best practices.
    - Auto-provisioning enabled by default when Defender for Containers is activated.
        
- Supports enforcement mode to block non-compliant actions.
    
    - E.g., blocking creation of privileged containers.

2. **Vulnerability Assessment**

- Scans container images in:
    
    - Azure Container Registry (ACR)
        
    - AKS clusters
- *Kubernetes audit logs* from **Azure Container Registry (ACR)** are scanned.

	- Non-ACR images appear under “Not applicable”.
        
- Identifies vulnerabilities pre- and post-deployment.
   
 3. **Run-Time Threat Protection**

- Monitors:
    
    - Kubernetes audit logs
    - Defender security profile
    - Linux nodes
- Examples of Detected Events:
	
	- Exposed Kubernetes dashboards
	- Creation of high-privileged roles
	- Creation of sensitive mounts
- AI and anomaly-based detections.
- Continuously updated by Microsoft’s security research team.

>**Amazon EKS** – Requires AWS account to be connected via _Microsoft Defender for Cloud > Environment Settings_. CSPM plan must be enabled.

---
## 9. Defender additional protections

### **1. Network Layer Threat Protection**

- Defender for Cloud uses *IPFIX sample data* (packet headers) from Azure core routers.
    
- Applies **machine learning models** to detect:
    
    - Malicious traffic patterns
        
    - Suspicious behavior
        
- **Microsoft Threat Intelligence** is used to enrich and validate IP addresses.
    

*Requirements for Network Threat Detection:*

- The VM must:
    
    - Have a **public IP address**
        
    - Or be behind a **load balancer with a public IP**
        
- **Network egress traffic** must not be blocked by an external IDS.

### **2. Azure Cosmos DB Threat Protection (Preview)**

- Detects **unusual or harmful attempts** to access or exploit Cosmos DB accounts.
    
- Alerts are generated based on abnormal behavior patterns.

### **3. Azure WAF Alerts in Defender for Cloud**

- Azure Application Gateway WAF provides **centralized protection** against:
    
    - Common exploits
        
    - Known web application vulnerabilities
        
- Uses **OWASP Core Rule Set**:
    
    - Version 3.0 or 2.2.9
        
- **Automatic rule updates** ensure up-to-date protection.
    
- If licensed, **WAF alerts are streamed to Defender for Cloud** automatically — no extra setup needed.

### 4. Azure DDoS Protection Alerts in Defender for Cloud

- Azure DDoS Protection license (multiple service tiers)
	
- **Cloud-native best practices** in application design
        
- When enabled, DDoS alerts are displayed in Defender for Cloud.

### **5. Defender for Cloud Recommendations in Defender for Cloud Apps**

- **Microsoft Defender for Cloud Apps** is a Cloud Access Security Broker (CASB) that supports:
    
    - Log collection
    - API connectors
    - Reverse proxy deployment

- Provides:
    - Visibility and control over cloud data
    - Threat detection across Microsoft and third-party cloud apps
        
- If **integration is enabled**, Defender for Cloud **automatically shares hardening recommendations** with Defender for Cloud Apps — no manual setup required.