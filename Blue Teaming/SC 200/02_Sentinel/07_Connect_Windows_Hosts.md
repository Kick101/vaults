### Option 1: Windows Security Events Content Hub Solution

- Provides **two agent-based connector options**:
    
    - **Windows Security Events via AMA Connector** _(Recommended)_
        
    - **Security Events via Legacy Agent Connector**
        
- Data is streamed *directly from the Windows device* to Microsoft Sentinel.

### Option 2: Windows Forwarded Events Content Hub Solution

- Requires configuration of a **Windows Event Collector (WEC)**.
- Devices forward events to *WEC*, which *then forwards them to Sentinel* using the Windows Forwarded Events data connector.
- WEF provides **agentless forwarding**, using built-in Windows functionality.

### AMA Connector vs. Legacy Agent Connector

__Advantages of AMA Connector__

- Centralized collection settings management
- Uses the **Azure Monitoring Agent (AMA)** – shared across multiple monitoring solutions    
- Improved performance and security

__⚠️ Limitations__

- **None**, except in environments where **AMA is unsupported**
- **Non-Azure VMs/devices** require **Azure Arc** integration

### Azure Arc Overview

- **Azure Arc** allows **non-Azure** devices/VMs to be managed like native Azure VMs.
    
- Uses the *Azure Connected Machine agent (azcmagent)*
    
- Enables:
    
    - Centralized management
        
    - Hybrid deployments of Azure services

---
## Connect with AMA Connector

The *Windows Security Events* via *AMA Connector uses Data Collection Rules* (DCRs) to define the data to collect from each agent. 

Data collection rules offer you two distinct advantages:

- **Manage collection settings at scale** while still allowing unique, scoped configurations for subsets of machines.
    
- **Build custom filters** to choose the exact events you want to ingest. The Azure Monitor Agent uses these rules to *filter the data at the source* and ingest only the events you want.

### Prerequisites

- Read and write permissions on the Sentinel workspace.
- To collect events from any system that *isn't an Azure virtual machine*, the system must have *Azure Arc* installed and enabled *before* you enable the Azure Monitor Agent-based connector.

These include:

- Windows servers installed on physical machines
- Windows servers installed on on-premises virtual machines
- Windows servers installed on virtual machines in non-Azure clouds



### Steps to Connect Windows Hosts

1. **Open Connector**
    
    - Go to **Microsoft Sentinel → Data connectors**
        
    - Select **Windows Security Events via AMA**
        
    - Click **Open connector page**
        
2. **Verify Prerequisites**
    
    - Confirm permission and Azure Arc status (for non-Azure systems)
        
3. **Create Data Collection Rule (DCR)**
    
    - Under _Configuration_, select **+ Add data collection rule**
        
4. **Configure Basics**
    
    - Specify:
        
        - **Rule Name**
            
        - **Subscription**
            
        - **Resource Group**  
            _(Can differ from that of the monitored machine; must be in the same tenant)_
            
5. **Select Resources**
    
    - Click **+ Add resource(s)**
        
    - Browse by subscription → resource group → machine
        
    - Select:
        
        - Individual machines
            
        - Entire resource groups or subscriptions
            
    - **Apply** your selection
        
    - **Azure Monitor Agent** is automatically installed if not present
        
6. **Define Event Collection Scope**
    
    - Choose one of the following:
        
        - **All Security Events**
            
        - **Common**
            
        - **Minimal**
            
        - **Custom** (for XPath filters)
            
    - **Custom Option Details**:
        
        - Supports **XPath v1.0 only**
            
        - Up to **20 expressions per box**
            
        - Up to **100 boxes per rule**
            
7. **Review and Create**
    
    - Click **Next: Review + create**
        
    - On successful validation, select **Create**

### Managing DCRs

- All DCRs (including API-created) are listed under **Configuration** in the connector page
    
- Options available:
    
    - **Edit**
    - **Delete**


### Validate XPath Queries (Optional Testing)

```powershell
$XPath = '*[System[EventID=1035]]'
Get-WinEvent -LogName 'Application' -FilterXPath $XPath
```



Output Interpretation:

- If events return → query is valid

- If "No events found" → query may be valid, but no matching data exists

- If "The specified query is invalid" → syntax error in XPath

---
## Collect Sysmon event logs

### Collection with Microsoft Sentinel

- Once Sysmon is running on endpoints:
    
    - Install the **Windows Forwarded Events Content Hub solution**
        
    - Leverage **Windows Forwarded Events data connector** to ingest *Sysmon* logs via **Azure Monitor Agent (AMA)**

### Install the Content Hub Solution

- Go to:
    
    - Azure portal -> Microsoft Sentinel -> Content management -> Content hub
        
    - Defender portal -> Microsoft Sentinel -> Content management -> Content hub
        
- Search for **Windows Forwarded Events**
    
- Select **Install**

```xml
Microsoft-Windows-Sysmon/Operational!*
```
### ASIM Support

- This connector supports **Advanced Security Information Model (ASIM)**
    
- Microsoft recommends enabling ASIM for normalization and standardized analytics