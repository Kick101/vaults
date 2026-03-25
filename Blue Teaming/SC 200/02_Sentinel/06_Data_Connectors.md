**Data ingestion** into Microsoft Sentinel is enabled by configuring **data connectors**.

These connectors are bundled within **Content Hub solutions**, supporting:

- **Microsoft services and products**
	
- **Third-party solutions**

>#### **Important Notes**
>
>- **Data Connectors cannot be deleted** once installed.
>- They can only be **disconnected or deactivated** as needed.

### Vendor connectors

Microsoft Sentinel provides an ever-growing list of vendor-specific data connectors. These connectors primarily use the CEF and Syslog connector as their foundation.

### Custom connectors using the Log Analytics API

You can use the Log Analytics Data Collector API to send log data to the Microsoft Sentinel Log Analytics workspace.


### Logstash Output Plugin for Microsoft Sentinel

- Enables forwarding of arbitrary logs from **Logstash** to a **Log Analytics workspace** in **Microsoft Sentinel**.
- Uses Microsoft Sentinel's **output plugin for Logstash**.
        
- Allows you to define a **custom table** for log ingestion.
	
- Logs are written **directly to this custom table** in Log Analytics.

---

>Use the **generic CEF or Syslog connector** when no vendor-provided connector is available.

To connect a **CEF or Syslog collector** to Microsoft Sentinel, you need to deploy the *Log Analytics agent* on a dedicated system.
### **Syslog Connector**

- Protocol used mainly on *Linux* systems for event logging.
    
- Applications send logs to:
    
    - **Local machine**
        
    - **Syslog collector**
        
- Logs are ingested into the *Syslog table* in Microsoft Sentinel.
    
    - Key field: `SyslogMessage` (contains raw, unparsed log data).
        
    - **Manual parsing** is required to extract usable fields.
        

### **Common Event Format (CEF) Connector**

- **Syslog-based industry standard** used by many security vendors.
    
- Ingested into the *CommonSecurityLog* table.
    
- **Preferred over plain Syslog** because:
    
    - Log data is automatically **parsed into predefined fields**.
        
    - Enables **easier querying and analysis**.

### Deployment Methods

**Automatic Deployment**:
	
- Available only for:
	
	- Azure VMs
		
	- Azure Arc–enabled servers
			
**Manual Deployment**:
	
- Required for:
	
	- On-premises systems
		
	- Third-party cloud VMs
		
	- Existing unmanaged Azure VMs

----

### Architecture Scenarios

- **Scenario 1: Azure-based architecture**
    
    - On-premises systems send Syslog/CEF data to a **dedicated Azure VM**.
        
    - Azure VM runs the **Log Analytics agent**.
        
    - Data is forwarded to **Microsoft Sentinel**.
        
- **Scenario 2: On-premises architecture**
    
    - On-premises systems send Syslog/CEF data to a **dedicated on-premises machine**.
        
    - Machine runs the **Log Analytics agent**.
        
    - Data is sent to **Microsoft Sentinel** from the on-premises system.
---
### 1. Microsoft 365 Connector

- **Configuration options**:
    
    - Choose to ingest data from:
        
        - **Exchange**
            
        - **SharePoint**
            
        - **Teams**
            
- **Data type**:
    
    - All data is ingested into the *OfficeActivity* table.

- **Data examples**:
    
    - File downloads
        
    - Access requests
        
    - Group modifications
        
    - `Set-Mailbox` operations
        
    - Actor (user) details
---
### 2. Microsoft Entra ID Connector

- **Options available**:
    
    - **Sign-In logs**
        
    - **Audit logs**
        
- **Target tables** depend on selected log type.
    
- **Sign-in logs**:
	
	- Application usage
		
	- Conditional access policy outcomes
		
	- Legacy authentication usage
		
- **Audit logs**:
	
	- Self-Service Password Reset (SSPR) activity
		
	- Microsoft Entra management (user, group, role, app)
    
---
### **3. Microsoft Entra ID Protection Connector**

- Sends alerts to the *SecurityAlert* table. 

- **Purpose**: Provides a unified view of:
    
    - At-risk users
        
    - Risk events
        
    - Vulnerabilities
    
- **Recommended configuration**:
    
    - Enable **Create Incidents** to automatically generate incidents from alerts.
    - Incident creation rules can be managed on the **Analytics** page.

        
- **Key Features**:
    
    - Immediate risk remediation
        
    - Autoremediation policy configuration
        
    - Optional automatic incident creation from alerts


---
### 4. Azure Activity Connector

- **Data source**:
    
    - Azure subscription-level **Activity Logs**.
        
- **Content includes**:
    
    - **Azure Resource Manager (ARM)** operations
        
    - **Service health events**
        
    - **Write operations** on Azure resources
        
    - **Status of performed activities**

---