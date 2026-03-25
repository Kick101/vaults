*Azure Arc* delivers **consistent governance and management** for hybrid and multicloud environments by projecting non-Azure resources into Azure.

### **Azure Arc Capabilities**

- Manage all environments through **a single Azure interface (Resource Manager)**.
    
- Treat non-Azure **VMs**, **Kubernetes clusters**, and **databases** like native Azure resources.
    
- Use **Azure services and policies** across any location.
    
- Combine **traditional ITOps** with **DevOps practices** to support cloud-native workflows.
    
- Use **Custom Locations** as an abstraction layer on top of Arc-enabled kurenetes cluster:
    
    - Arc-enabled Kubernetes clusters
        
    - Cluster connect
        
    - Cluster extensions

### **Supported Non-Azure Resource Types**

- **Servers** – Physical or virtual, Windows or Linux
    
- **Kubernetes clusters** – Supports multiple distros
    
- **Azure data services** – Azure SQL Managed Instance, PostgreSQL Hyperscale
    
- **SQL Server** – Enroll instances from any location using Arc-enabled servers


### What Is a Custom Location?

A **Custom Location** acts as a **bridge** between:

- **Azure services** (e.g., App Services, Azure Functions, Logic Apps)
- **Azure Arc-enabled Kubernetes clusters** running outside of Azure

It tells Azure:

> “Treat this specific Kubernetes environment (that is not in Azure) as if it's an Azure region or deployment target.”

---
## Connection Methods

1. Azure Arc-enabled servers (recommended)
    
2. Azure Arc-enabled VMware vSphere
    
3. Azure Arc-enabled SCVMM (System Center Virtual Machine Manager)
    
4. Azure Local

### Azure Arc-Enabled Servers (Preferred Method)

- Converts non-Azure machines into **Azure resources**
    
- **Enables**:
    
    - Security recommendations in Defender for Cloud
        
    - Guest configuration policies (A feature of Azure Policy that **audits or enforces settings inside the *guest OS***)
        
    - Azure Monitor Agent deployment
        
    - Integration with Azure services

### What Happens When Connected

- Machine gets:
    
    - A **Resource ID**
        
    - Inclusion in an **Azure Resource Group**
        
    - Access to **Azure constructs**: tags, policies, automation, etc.
        
- Managed **like native Azure VMs**, even across **multiple environments** (via *Azure Lighthouse*).

#### 1. Azure Connected Machine Agent (for connection)

- Must be installed on each machine.
- **Does not replace** the Azure Monitor Agent.
- Required for:
    - Connecting hybrid machines to Azure
    - Enabling management capabilities through Azure Arc

#### 2. Azure Monitor Agent (for monitoring)
    
- Monitor OS health and workloads
	
- Enable services like:
	
	- Update Management
		
	- Automation *runbooks*
		
	- Defender for Cloud
            
### Onboarding methods

 **Interactive Methods**

- Deployment script from Azure portal
    
- Windows Admin Center
    
- Azure Arc Setup for Windows Servers
    
- PowerShell (interactive or at scale)
    

**At-Scale Methods**

- PowerShell with service principal (non-interactive)
    
- PowerShell scripts with Configuration Manager
    
- Configuration Manager custom task sequence
    
- Group Policy
    
- Azure Automation Update Management
    
- Arc-enabled VMware vSphere
    
- Arc-enabled System Center Virtual Machine Manager (SCVMM)
    
- AWS EC2 onboarding via Azure Arc multicloud connector

### Cloud Operations

#### 1. Govern (Policies)

- Assign **Azure Machine Configuration policies** to audit internal settings.
    
- Pricing details available in the Azure Policy pricing guide.

#### 2. Protect

- Use **Microsoft Defender for Endpoint** via Defender for Cloud to:
    
    - Detect threats
        
    - Manage vulnerabilities
        
    - Monitor for security risks
        
    - Receive alerts and remediation recommendations
        
- Integrate with **Microsoft Sentinel** to:
    
    - Collect security events
        
    - Correlate them across data sources

#### 3. Configure

- **Azure Automation**:
    
    - Automate *repetitive tasks* with PowerShell & Python runbooks
        
    - Track changes in:
        
        - Installed software
            
        - Windows registry and files
            
        - Linux daemons
            
- **Azure Update Manager**:
    
    - Manage OS updates for both Windows and Linux
        
- **Azure Automanage**:
    
    - Automate setup/configuration of *Azure services* on Arc machines
        
- **VM Extensions**:
    
    - Perform post-deployment configuration tasks on non-Azure Windows/Linux machines

#### 4. Monitor

- Use **VM Insights** to:
    
    - Monitor OS performance        
    - Discover processes and dependency maps
    
- Use **Azure Monitor Agent** to:
    
    - Collect logs, performance data, and events
        
    - Store data in **Log Analytics workspace** with:
        
        - Machine-specific properties (e.g., Resource ID)
            
        - Resource-context for log access and analysis

---
## AWS Accounts



