### 🔌 **1. Data Connectors (Ingestion Layer)**

**Purpose:** Ingest logs and telemetry from a wide array of sources.

- **Plug-and-play connectors**: For services like *Azure Activity Logs* and Microsoft Defender.
    
- **Manual setup connectors**: For logs via:
    
    - `Syslog`
        
    - `Common Event Format (CEF)`
        
    - `TAXII` (for threat intelligence feeds)
        
    - AWS and GCP log ingestion
        

> **Note**: Most connectors are enabled via **Content Hub** solutions in the Azure portal.

### 🗃️ **2. Log Retention (Storage & Query Layer)**

- Data is stored in an **Azure Log Analytics Workspace**.
    
- You query data using **Kusto Query Language (KQL)**

> **Use case:** Conduct deep forensic investigations or create custom detections.

### 📊 **3. Workbooks (Visualization Layer)**

- **Workbooks = Dashboards** built using KQL queries.
    
- You can:
    
    - Use built-in workbooks
        
    - Customize existing ones
        
    - Build dashboards from scratch

### 🚨 **4. Analytics Alerts (Detection Layer)**

- Use **built-in analytics rules** (editable) and **ML-based detections** (proprietary to Microsoft).    
- Create **custom scheduled queries** to generate alerts based on your own logic.


> Example: Trigger an alert if a service account authenticates from multiple countries within 10 minutes.

### 🧭 **5. Threat Hunting**

- Use **built-in hunting queries** from Content Hub solutions.
    
- Write custom queries to proactively search for anomalies.
    
- Advanced threat hunters can use **Azure Notebooks** (*Jupyter-based*) for programmatic hunting.

### 🧵 **6. Incidents & Investigation**

- Multiple alerts can be **correlated into a single incident**.
    
- Perform tasks like:
    
    - Assigning ownership
        
    - Changing severity or status
        
- Use **visual incident graphs** to trace attack paths across entities (accounts, IPs, devices).
    

> A powerful way to understand lateral movement and attack timelines.

### ⚙️ **7. Automation Playbooks (SOAR Layer)**

- Create **Playbooks using Azure Logic Apps** to automate:
    
    - Enrichment (e.g., IP reputation lookups)
        
    - Response (e.g., disable user, isolate VM)
        
    - Notifications (e.g., send to Teams, email)
        
    - Remediation
        

> Automate both reactive (after alert) and proactive (scheduled actions) workflows.

---

