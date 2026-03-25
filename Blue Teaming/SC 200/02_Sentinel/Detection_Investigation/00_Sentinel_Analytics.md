- Analyzes historical data from sources such as:

	- Workstations
	    
	- Servers
	    
	- Networking devices
	    
	- Firewalls
	    
	- Intrusion prevention systems
	    
	- Sensors

- Identifies correlations and anomalies across multiple data sources.
- Combines multiple data sources with threat intelligence for better detection and mitigation.

**Provides insights into:**

- Attack origin
    
- Compromised resources
    
- Potential data loss
    
- Incident timeline

**Types of Analytics Rules**

- Anomaly
    
- Fusion
    
- Microsoft Security
    
- Machine Learning (ML) Behavior Analytics
    
- Scheduled Alerts
    
- NRT (Near Real Time) Rules
    
- Threat Intelligence

### Anomaly

- Informational alerts identifying anomalous behaviors.


### Fusion

- Uses *Fusion correlation engine* with scalable ML algorithms to *detect advanced multistage attacks*.
- Correlates low-fidelity alerts/events from multiple products into high-fidelity incidents.
- Enabled by default; logic is hidden and non-customizable (*only one rule per template*).
- Can correlate alerts from:
    - Scheduled analytics rules
    - Other systems

**Required Data Connectors**:
- Out-of-the-box anomaly detections
- Alerts from Microsoft products            
- Alerts from scheduled analytics rules (*must contain kill-chain tactics + entity mapping*).

**Common Detection Scenarios**:
- **Data exfiltration** – e.g., suspicious mailbox forwarding after suspicious sign-in.
- **Data destruction** – e.g., unusual file deletion after suspicious sign-in.
- **Denial of service** – e.g., Azure VMs deleted after suspicious sign-in.
- **Lateral movement** – e.g., impersonation actions after suspicious sign-in.
- **Ransomware** – e.g., unusual behavior encrypting data after suspicious sign-in.

### Microsoft Security

- *Connect Microsoft security solutions to Sentinel* to auto-create incidents from all generated alerts.
- Example: Alert when a high-risk user attempts sign-in to corporate resources.       
- Alerts can be filtered by severity and alert name text.
- Microsoft unifies SIEM + XDR terminology across its security products.

### ML Behavior Analytics

- Built-in ML-based analytics rules (*non-editable*, settings not viewable).
- Detect suspicious activity by *correlating low-fidelity incidents into high-fidelity incidents*.
- Reduces alert noise by automatically connecting important data.
- Example: Detect anomalous SSH or RDP sign-in activity.
    
### Scheduled Alerts

- *Most customizable* analytics rule type.
- Define *custom KQL expressions* to filter security events.
- Configure *schedule for rule execution*.

---
## Create an analytics rule from templates


>When you implement filters to include or exclude specific alerts based on a text string, these alerts will not appear in Microsoft Sentinel.


> *don't search for data that is older than the query's run frequency* because that can create duplicate alerts








