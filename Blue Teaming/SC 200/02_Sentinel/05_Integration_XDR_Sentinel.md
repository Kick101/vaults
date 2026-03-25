### #1. Ingest Defender XDR Data into Microsoft Sentinel

- **Functionality**:
    
    - Collects data from Microsoft Defender XDR services into Sentinel.
        
    - Enables analysis and investigation using Sentinel’s analytics, automation, and workbooks.
        
- **Access Point**:
    
    - View Microsoft Defender XDR data in the **Azure portal** via Sentinel.
        
- **Enablement**:
    
    - Install the **Microsoft Defender XDR connector** in Microsoft Sentinel.

### #2. Integrate Sentinel into the Defender Portal (Unified Operations Platform)

- **Functionality**:
    
    - Centralizes Sentinel data within the **Microsoft Defender portal**.
        
    - Unifies incidents, alerts, vulnerabilities, and other data in a single interface.
        
- **Access Point**:
    
    - View Microsoft Sentinel data **alongside Defender content** in the **Defender portal**.
        
- **Enablement**:
    
    - Install the **Microsoft Defender XDR connector** in Sentinel.
        
    - **Onboard Sentinel** to the unified operations platform via the Defender portal.

---

Most Microsoft Sentinel capabilities are available in both the Azure and Defender portals. In the Defender portal, some Microsoft Sentinel experiences open out to the Azure portal for you to complete a task.

### Capability differences between portals

| Capability                                                                  | Availability                                                                                                                                                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| --------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Advanced hunting using bookmarks                                            | Azure portal only                                                                                                                                                 | Bookmarks aren't supported in the advanced hunting experience in the Microsoft Defender portal. In the Defender portal, they're supported in the **Microsoft Sentinel > Threat management > Hunting**.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Attack disruption for SAP                                                   | Defender portal only                                                                                                                                              | This functionality is unavailable in the Azure portal.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Automation                                                                  | Some automation procedures are available only in the Azure portal. And Other automation procedures are the same in the Defender and Azure portals.                | The differences in the Azure portal are between workspaces that are onboarded to the Microsoft Defender portal and workspaces that aren't.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Data connectors: visibility of connectors used by Microsoft Defender        | Azure portal only                                                                                                                                                 | In the Defender portal, after you onboard Microsoft Sentinel, the following data connectors that are part of Microsoft Defender aren't shown in the **Data connectors** page:- Microsoft Defender for Cloud Apps<br>- Microsoft Defender for Endpoint<br>- Microsoft Defender for Identity<br>- Microsoft Defender for Office 365 (Preview)<br>- Microsoft Defender XDR<br>- Subscription-based Microsoft Defender for Cloud (Legacy)<br>- Tenant-based Microsoft Defender for Cloud (Preview)  <br>      <br>    In the Azure portal, these data connectors are still listed with the installed data connectors in Microsoft Sentinel. |
| Entities: Add entities to threat intelligence from incidents                | Azure portal only                                                                                                                                                 | This functionality is unavailable in the Microsoft Defender portal.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Fusion: Advanced multistage attack detection                                | Azure portal only                                                                                                                                                 | The Fusion analytics rule, which creates incidents based on alert correlations made by the Fusion correlation engine, is disabled when you onboard Microsoft Sentinel to Microsoft Defender.  <br>  <br>Microsoft Defender uses Microsoft Defender XDR's incident-creation and correlation functionalities to replace those of the Fusion engine.                                                                                                                                                                                                                                                                                       |
| Incidents: Adding alerts to incidents /  <br>Removing alerts from incidents | Defender portal only                                                                                                                                              | After onboarding Microsoft Sentinel to the Microsoft Defender portal, you can no longer add alerts to, or remove alerts from, incidents in the Azure portal.  <br>  <br>You can remove an alert from an incident in the Defender portal, but only by linking the alert to another incident (existing or new).                                                                                                                                                                                                                                                                                                                           |
| Incidents: editing comments                                                 | Azure portal only                                                                                                                                                 | After onboarding Microsoft Sentinel to the Microsoft Defender portal, you can add comments to incidents in either portal, but you can't edit existing comments.  <br>  <br>Edits made to comments in the Azure portal don't synchronize to the Microsoft Defender portal.                                                                                                                                                                                                                                                                                                                                                               |
| Incidents: Programmatic and manual creation of incidents                    | Azure portal only                                                                                                                                                 | Incidents created in Microsoft Sentinel through the API, by a Logic App playbook, or manually from the Azure portal, aren't synchronized to the Microsoft Defender portal. These incidents are still supported in the Azure portal and the API.                                                                                                                                                                                                                                                                                                                                                                                         |
| Incidents: Reopening closed incidents                                       | Azure portal only                                                                                                                                                 | In the Microsoft Defender portal, you can't set alert grouping in Microsoft Sentinel analytics rules to reopen closed incidents if new alerts are added.  <br>Closed incidents aren't reopened in this case, and new alerts trigger new incidents.                                                                                                                                                                                                                                                                                                                                                                                      |
| Incidents: Tasks                                                            | Azure portal only                                                                                                                                                 | Tasks are unavailable in the Microsoft Defender portal.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Multiple workspace management for Microsoft Sentinel                        | Defender portal: Limited to one primary Microsoft Sentinel workspace  <br>  <br>Azure portal: Centrally manage multiple Microsoft Sentinel workspaces for tenants | Only one primary Microsoft Sentinel workspace can be connected in the Microsoft Defender portal. So, Microsoft Defender multitenant management supports one primary Microsoft Sentinel workspace per tenant.                                                                                                                                                                                                                                                                                                                                                                                                                            |

---
## Onboarding Sentinel to Defender XDR

### Prerequisites for Using Sentinel in the Defender Portal

- **Tenant Scope**:
    
    - The Microsoft Defender portal supports a **single Microsoft Entra tenant**.
        
    - It allows connection to:
        
        - One **primary Log Analytics workspace**
            
        - Multiple **secondary workspaces**
            
    - Each workspace must have **Microsoft Sentinel enabled**.
- **Enable** the _Microsoft Defender XDR data connector_ to collect incidents and alerts.

#### **Required Resources and Access**

- A **Log Analytics workspace** with **Microsoft Sentinel enabled**
    
- **Microsoft Defender XDR data connector** enabled in Sentinel for ingesting incidents and alerts    
- Access to **Microsoft Defender XDR** via the **Defender portal**
    
- Microsoft Defender XDR must be **onboarded to the Microsoft Entra tenant**
    
- An **Azure account** with appropriate **RBAC roles** for onboarding and ongoing use


### Role-Based Access Requirements

| **Task**                                      | **Required Azure Built-In Role(s)**                                                                                                                                                                                                                                                                                                                                       | **Scope**                                                                                                                       |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Connect/disconnect Sentinel-enabled workspace | `Owner` or `User Access Administrator` **and** `Microsoft Sentinel Contributor`                                                                                                                                                                                                                                                                                           | - `Owner`/`User Access Admin`: **Subscription**- `Sentinel Contributor`: **Subscription**, **Resource Group**, or **Workspace** |
| View Microsoft Sentinel in Defender portal    | `Microsoft Sentinel Reader`                                                                                                                                                                                                                                                                                                                                               | **Subscription**, **Resource Group**, or **Workspace**                                                                          |
| Query data tables or view incidents           | `Microsoft Sentinel Reader` **or** custom role with:- `Microsoft.OperationalInsights/workspaces/read`- `Microsoft.OperationalInsights/workspaces/query/read`- `Microsoft.SecurityInsights/incidents/read`- `Microsoft.SecurityInsights/incidents/comments/read`- `Microsoft.SecurityInsights/incidents/relations/read`- `Microsoft.SecurityInsights/incidents/tasks/read` | **Subscription**, **Resource Group**, or **Workspace**                                                                          |
| Take investigative actions on incidents       | `Microsoft Sentinel Contributor` **or** custom role with:- All permissions listed above plus:- `Microsoft.SecurityInsights/incidents/write`- `Microsoft.SecurityInsights/incidents/comments/write`- `Microsoft.SecurityInsights/incidents/relations/write`- `Microsoft.SecurityInsights/incidents/tasks/write`                                                            | **Subscription**, **Resource Group**, or **Workspace**                                                                          |
| Create support requests                       | `Owner`, `Contributor`, `Support Request Contributor`, or custom role with `Microsoft.Support/*`                                                                                                                                                                                                                                                                          | **Subscription**                                                                                                                |

### **Role Management Notes**

- Your existing **Azure RBAC permissions** apply in the **Defender portal** after connecting Microsoft Sentinel.
    
- Continue managing Sentinel roles from the **Azure portal**.
    
- Any RBAC changes are **automatically reflected** in the Defender portal.

### Steps to Connect a Sentinel Workspace to Defender XDR

1. **Sign in** to the Microsoft Defender portal.
    
2. Navigate to **Home (Overview)** in Microsoft Defender XDR.
    
3. Select **“Connect a workspace”** from the _Get your SIEM and XDR in one place_ banner.
    
4. Choose the **Log Analytics workspace** with Microsoft Sentinel enabled, then select **Next**.
    
5. **Review the product behavior changes** upon connection:
    
    - Sentinel **log tables, queries, and functions** become available in **Defender XDR advanced hunting**.
        
    - The *Microsoft Sentinel Contributor* role is automatically assigned to:
        
        - `Microsoft Threat Protection` app
            
        - `WindowsDefenderATP` app
            
    - **Incident creation rules** for Microsoft alerts in Sentinel are **deactivated** to prevent duplication.
        
        > Other analytics rules remain unaffected.
        
    - **All Defender alerts** are streamed directly via the **Defender XDR connector**, ensuring centralized incident ingestion.
        
6. Select **Connect**.
    

> *Once completed:*
> - The **Overview page** reflects **Microsoft Sentinel metrics**, including data connectors and automation rules.
> - The banner confirms unified **SIEM + XDR** integration is active.


---
## **Offboarding Microsoft Sentinel from Defender XDR**

> **Note**: Only **one workspace** can be connected at a time. To switch workspaces, disconnect the current one.

### **Steps to Disconnect a Workspace**

1. Go to the Microsoft Defender portal and sign in.
    
2. Navigate to:  
    **Settings > Microsoft Sentinel** under the **System** section.
    
3. On the **Workspaces** page, select the **currently connected workspace**.
    
4. Click **Disconnect workspace**.
    
5. Provide a **reason for disconnection** and confirm.
    

> *After disconnection:*
> - The **Microsoft Sentinel section** is removed from the Defender portal navigation.
> - **Sentinel data** no longer appears on the Overview page.

### **To Connect a Different Workspace**

- Return to the **Workspaces** page.
    
- Select the new workspace and click **Connect a workspace**.
    
- Follow the onboarding steps as described above.