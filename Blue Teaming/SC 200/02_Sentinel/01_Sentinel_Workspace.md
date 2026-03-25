**The three implementation options:**

- Single-Tenant with a single Microsoft Sentinel Workspace
    
- Single-Tenant with regional Microsoft Sentinel Workspaces
    
- Multiple tenants

### 1. Single-tenant single workspace

![[Pasted image 20250731154937.png |300]]

> Centralized approach — one Sentinel workspace across all regions and resources in a single Azure tenant.

|Pros|Cons|
|---|---|
|Central Pane of Glass|May not meet Data Governance Requirements|
|Consolidates all security logs and information|Can incur bandwidth cost for cross region|
|Easier to query all information||
|Azure Log Analytics RBAC to control data access||
|Microsoft Sentinel RBAC for service RBAC||
### Single-tenant with regional Sentinel workspaces

> Each region has its own Sentinel + Log Analytics workspace to meet compliance or performance needs.

![[Pasted image 20250731155046.png]]

|Pros|Cons|
|---|---|
|No cross-region bandwidth costs|No central pane of glass. You aren't looking in one place to see all the data.|
|May be required to meet Data Governance requirements|Analytics, Workbooks, etc. must be deployed multiple times.|
|Granular data access control||
|Granular retention settings||
|Split billing|

>To query data across workspaces, use the workspace() function before the table name.

```sql
TableName

| union workspace("WorkspaceName").TableName
```

### Multiple tenant workspaces

>Use **Azure Lighthouse** to manage Sentinel across **multiple tenants**.

![[Pasted image 20250731155309.png]]

- *Azure Lighthouse* allows delegated access across tenants.
    
- Workspace architecture within each tenant (centralized or regional) remains the same.

**Key Use Case**:

- **MSSPs** or enterprise security teams managing **multi-organization environments**.

---
## Create a Microsoft Sentinel workspace

### Prerequisites

- **Azure Role Requirements**:
    
    - To **enable Sentinel**: Contributor on the _subscription_.
        
    - To **use Sentinel**: Contributor or Reader on the _resource group_.
        
- **Billing**:
    
    - Creating or enabling Microsoft Sentinel may incur charges.
        
    - Refer to: Microsoft Sentinel Pricing

### 1. **Launch Microsoft Sentinel in Azure**

- Sign in to the Azure Portal.
    
- Search for **"Sentinel"** in the search bar.
    
- Select **Microsoft Sentinel**.
    
- Click **+ Add** to begin the creation process.
    

### 2. **Create a Log Analytics Workspace**

- On the **"Add Microsoft Sentinel to a workspace"** screen:
    
    - Click **+ Create a new workspace**.
        
- In the **Basics** tab:

|Field|Description|
|---|---|
|Subscription|Select your Azure subscription|
|Resource Group|Choose or create a resource group|
|Name|Name of the Log Analytics workspace (also used by Sentinel)|
|Region|Region where log data is stored (critical for compliance)|

### 3. **Attach Sentinel to the Workspace**

- Wait for your new workspace to appear (may take a few minutes).
- Select the **new Log Analytics workspace** from the list.
- Click **Add**.

---
## Manage workspaces across tenants using Azure Lighthouse

If you're required to manage *multiple Sentinel workspaces*, or workspaces not in your tenant, you have two options:

- Microsoft Sentinel Workspace manager
- Azure Lighthouse

### Microsoft Sentinel Workspace manager

- Enables users to centrally manage multiple Sentinel workspaces within one or more Azure tenants.
- Hosts and manages shared **content items** (e.g., analytics rules, workbooks).
- Publishes these at scale to **Member Workspaces**.

![[Pasted image 20250731181405.png]]

### Azure Lighthouse
- **Directory + Subscription Selector**:
    
    - Lets you view and manage **all workspaces** across delegated tenants from a single Azure portal session.
        
- **Multi-Tenant Operations**:
    
    - Ideal for **Managed Security Service Providers (MSSPs)** or organizations managing multiple business units or clients.
        
    - Eliminates the need to **sign in to multiple accounts** or switch tenants.

>*Delegated tenants* refer to **external Azure Active Directory (Azure AD) tenants** (i.e., customers or other organizations) that **grant access rights** to their resources (like Microsoft Sentinel workspaces) to a **managing tenant** — usually through **Azure Lighthouse**.

>A service provider can manage Sentinel workspaces for two customers, each with different access controls, from their own tenant using Azure Lighthouse.

![[Pasted image 20250731181501.png]]

---
## Microsoft Sentinel roles and allowed actions

>All Sentinel roles **grant read access** to data, but differ in what **additional actions** users can take:



| Roles                                                    | Create and run playbooks | Create and edit workbooks, analytic rules, and other Microsoft Sentinel resources | Manage incidents such as dismissing and assigning | View data incidents, workbooks, and other Microsoft Sentinel resources |
| -------------------------------------------------------- | ------------------------ | --------------------------------------------------------------------------------- | ------------------------------------------------- | ---------------------------------------------------------------------- |
| Microsoft Sentinel Reader                                | No                       | No                                                                                | No                                                | Yes                                                                    |
| Microsoft Sentinel Responder                             | No                       | No                                                                                | Yes                                               | Yes                                                                    |
| Microsoft Sentinel Contributor                           | No                       | Yes                                                                               | Yes                                               | Yes                                                                    |
| Microsoft Sentinel Contributor and Logic App Contributor | Yes                      | Yes                                                                               | Yes                                               | Yes                                                                    |
>*Microsoft Sentinel Automation Contributor*: allows Microsoft Sentinel to add playbooks to automation rules. It isn't meant for user accounts.

>⚠️ **Security Pitfall:** If you give someone `Azure Contributor` without restricting Microsoft Sentinel access specifically, they can **bypass RBAC constraints**.

### Additional roles and permissions

- **Playbooks (SOAR):** Built on Azure Logic Apps. Assign `Logic App Contributor` for editing.
    
- **Run Playbooks:** Sentinel uses a managed service account. Needs explicit permission on the playbook's resource group. Requires `Owner` role to assign.
    
- **Connect Data Sources:** Connectors Requires write permission on Sentinel workspace. Some connectors need extra permissions.
    
- **Guest Users Assigning Incidents:** Needs `Microsoft Sentinel Responder` + `Directory Reader` (Entra role).
    
- **Create/Delete Workbooks:** Requires `Sentinel Contributor` or lower Sentinel role + `Workbook Contributor`.

### Azure & Log Analytics roles

- **Azure roles** (apply across all resources):
    
    - `Owner`, `Contributor`, `Reader`
        
    - Affect Sentinel + Log Analytics
        
- **Log Analytics roles** (Log Analytics only):
    
    - `Log Analytics Contributor`, `Log Analytics Reader`
        

> ⚠️ If a user has `Sentinel Reader` + `Azure Contributor`, they can still edit Sentinel data.  
> Clean up extra roles to avoid unwanted access.

---
### **Manage Microsoft Sentinel Settings**

- Microsoft Sentinel settings are managed in two locations:
    
    - Within Microsoft Sentinel itself.
        
    - In the Log Analytics workspace where Microsoft Sentinel is deployed.
        
- In Microsoft Sentinel:
    
    - Access settings via the **left navigation > Settings**.
        
    - The **Settings** section includes:
        
        - **Pricing**
            
        - **Settings**
            
        - **Workspace Settings**
            
    - This section evolves based on active and preview features.
        
- Most environment-level configurations are made in the **Log Analytics workspace**.
    
- Certain actions (e.g., *configuring data connectors*) will redirect you from Sentinel to the **Log Analytics portal**.

### **Configure Log Retention**

- **Retention period** at the workspace level:
    
    - Can be set between **30 and 730 days (2 years)**.
        
    - Not configurable for workspaces on the **legacy Free pricing tier**.
        
- To change retention settings:
    
    1. Go to **Microsoft Sentinel > Settings > Workspace settings**.
        
    2. You'll be redirected to the **Log Analytics portal**.
        
    3. Click on the "*Usage and estimated costs*" tab.
        
    4. Select the **"Retention"** button at the top.
        
    5. Adjust the retention period in the window that appears.
        

---

## Configure Logs
#### **Primary Log Types in Microsoft Sentinel**

1.  **Analytics Logs**
    
    - Default type for all tables.
        
    - Full feature access for queries, visualizations, alerts.
        
    - Retention: 30–730 days (can be extended to 2 years for interactive access).
        
2. **Basic Logs**
    
    - Used for high-volume, verbose logs (debugging, auditing).
        
    - Lower cost, reduced features.
        
    - Fixed retention: **8 days**.
        
    - **Query limitations**: Supports a subset of KQL (e.g., `where`, `project`, `parse`)
        
        - _Not supported_: `join`, `union`, `summarize`.
            
3.  **Auxiliary Logs** _(Preview)_
    
    - For low-touch data, compliance/audit logs.
        
    - Ingested at low cost.
        
    - Fixed retention: **30 days**.
        
    - Basic query support only.

**To access archived data, you must first retrieve data from it in an Analytics Logs table using one of the following methods:**

- Search Jobs
- Restore

>By default, all tables in a workspace are of type Analytics Logs
### Configuring Log Type

To set a table as **Basic** or **Auxiliary**:

1. Go to **Microsoft Sentinel > Settings > Workspace Settings**.
    
2. In the **Log Analytics portal**, go to the **Tables** tab.
    
3. Select a table → Click **“...”** at the row end → **Manage table**.
    
4. Change the **Table Plan**.
    
5. Click **Save**.
    

**Only eligible tables** (not used by Azure Monitor features) can be switched to Basic.  
Examples:

- DCR-based custom logs
    
- `ContainerLogV2` (Container Insights)
    
- `AppTraces` (Application Insights)

### **Retention Configuration**

#### **Interactive Retention (default)**

- Default: **30 days**
    
- Some log tables: **90 days**
    
- For **Analytics plan**, extendable to **2 years**
    
- **Basic/Auxiliary logs**: fixed to 8/30 days respectively
    

#### **Long-Term Retention**

- Extend total retention to **up to 12 years**
    
- Data beyond interactive period is archived
    
- Use **search job** to retrieve archived data into a results table for querying


### Steps to Configure Table Retention

1. Go to **Microsoft Sentinel > Settings > Workspace Settings**.
    
2. In **Log Analytics portal**, open **Tables** tab.
    
3. Select a table → Click **“...”** → **Manage table**.
    
4. Adjust **Total Retention Period**.
    
5. Click **Save**.
