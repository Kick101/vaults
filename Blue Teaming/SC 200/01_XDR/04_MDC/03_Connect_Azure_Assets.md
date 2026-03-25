## Asset Inventory

- All connected resources
- Their *security posture*
- Resources with unresolved recommendations appear directly in the inventory.

### Answering Questions via Filtered Views

- Identify subscriptions with **outstanding recommendations**
    
- Find machines tagged ‘Production’ that **lack the Log Analytics agent**
    
- Count how many tagged machines have **unresolved security issues**
    
- List resources in a resource group with **vulnerability assessment findings**

### Key Features

#### 1. Summaries

- Displayed before any filters are applied:
    
    - **Total resources**: Connected to Defender for Cloud
        
    - **Unhealthy resources**: Have active security recommendations
        
    - **Unmonitored resources**: Deployed with Log Analytics agent but **not sending data** or have health issues
        

#### 2. Filters

- Subscription
- Resource group
- Tags
- Specific security findings (ID, check name, CVE)
- Summary values update **dynamically** based on filter input

#### 3. Export Options

- **CSV file**
- **Azure Resource Graph Explorer** (ARG) for advanced KQL-based querying

#### 4. Asset Management Operations

- After filtering:
    
    - **Assign tags** to selected resources
        
    - **Add non-Azure servers** via toolbar
        
    - **Trigger Logic Apps** for automation (*Logic Apps* must be pre-configured to accept HTTP request triggers)

### How Asset Inventory Works

- Powered by **Azure Resource Graph (ARG)**:
    
    - Enables querying at scale across subscriptions
        
    - Uses **Kusto Query Language (KQL)**
        
- Combines security posture data from *ASC* (**Azure Security Center**) with resource metadata for deep insights

![[Pasted image 20250730123708.png]]
### How to Use Asset Inventory

1. In **Defender for Cloud**, go to **Inventory**
    
2. Use **Filter by name** or advanced filters
    
3. Apply filters by:
    
    - Subscription
        
    - Resource group
        
    - Tags
        
    - Security findings (ID, check name, CVE)
        
    - Defender for Cloud plan status
        

#### **Defender for Cloud Filter Options:**

- **Off**: Not protected (right-click to upgrade)
    
- **On**: Fully protected
    
- **Partial**: Some plans disabled on the subscription

---
## Auto provisioning

### Why Data Is Collected

- To detect:
    - Missing OS updates
    - Misconfigured security settings
    - Endpoint protection and threat status
- Required for full security visibility on compute resources.
- Agentless use is possible, but **security features will be limited**.

### **How Data Is Collected**

- **Log Analytics Agent**:
    
    - Gathers data like OS type/version, logs, running processes, IPs, usernames.
        
    - Sends to your **Log Analytics workspace**.
        
- **Security Extensions**:
    
    - E.g., *Azure Policy Add-on for Kubernetes*.
        
    - Collect specialized resource data for Security Center.

![[Pasted image 20250730175453.png]]

## **How Auto-Provisioning Works**

- Controlled via **toggle switches** in Defender for Cloud's **Environment settings**
    
- Enabling it applies a *DeployIfNotExists* Azure Policy to:
    
    - Automatically deploy the selected extension or agent
        
    - Cover all current and future supported resources

### Integration with Microsoft Sentinel

- **Only one platform** (Defender for Cloud _or_ Sentinel) should manage **Security Events** collection per workspace.
    

#### Option 1: Let Defender for Cloud manage

- Events visible in **both Sentinel and Defender for Cloud**
    
- **Cannot customize or monitor connector** from Sentinel
    

#### Option 2: Let Sentinel manage

- Set **Windows Security Events = None** in Defender for Cloud
    
- Use **Security Events connector** in Sentinel
    
- Enables connector configuration and monitoring in Sentinel

### Event Collection Tiers: Minimal vs Common

| Tier        | Purpose                            | Example Events Included                                            | Notes                                                         |
| ----------- | ---------------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------- |
| **Minimal** | Low-volume, breach-relevant events | - Logins: 4624, 4625- Process creation: 4688                       | Excludes noisy/audit-heavy events (e.g., sign-outs)           |
| **Common**  | Full user audit trail              | - Logins: 4624, 4625- Sign-outs: 4634- Group changes, Kerberos ops | Includes more audit data, still filters out high-volume noise |

----
