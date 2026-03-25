### **Enable Log Analytics Agent**

1. Go to **Environment settings** in Defender for Cloud.
    
2. Select the **target subscription**.
    
3. Open the **Auto provisioning** page.
    
4. Set **Log Analytics agent** to **On**.
    
5. Choose a **workspace** for data collection.

## **Auto-Provisioning Other Extensions**

1. In **Environment settings**, select the subscription.
    
2. Go to **Auto provisioning**.
    
3. Toggle desired extensions (e.g., Dependency Agent) to **On**.
    
    - *Note*: Log Analytics agent must also be enabled for some extensions.
        
4. Click **Save** to assign the Azure Policy and create a remediation task.

---
## Manual log analytics agent provisioning

To manually install the Log Analytics agent:

1. Disable auto provisioning.
    
2. Optionally, create a workspace.
    
3. Enable Microsoft Defender for Cloud on the workspace on which you're installing the Log Analytics agent:
    
4. From Defender for Cloud's menu, select **Environmental settings**.
    
5. Set the workspace on which you're installing the agent. Make sure the workspace is in the same subscription you use in Defender for Cloud, and you have read/write permissions for the workspace.
    
6. Select **Microsoft Defender for Cloud on**, and **Save**.
    

- To deploy agents on new VMs using a Resource Manager template, install the Log Analytics agent:
    
    - [Install the Log Analytics agent for Windows](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-windows)
        
    - [Install the Log Analytics agent for Linux](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-linux)
        
- To deploy agents on your existing VMs, follow the instructions in [Collect data about Azure Virtual Machines](https://learn.microsoft.com/en-us/azure/azure-monitor/learn/quick-collect-azurevm).
    
- To use PowerShell to deploy the agents, use the instructions from the virtual machines documentation:
    
    - [For Windows machines](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-windows?toc=%2Fazure%2Fazure-monitor%2Ftoc.json%3Fazure-portal%3Dtrue)
        
    - [For Linux machines](https://learn.microsoft.com/en-us/azure/virtual-machines/extensions/oms-linux?toc=%2Fazure%2Fazure-monitor%2Ftoc.json%3Fazure-portal%3Dtrue)

