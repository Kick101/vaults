## QRadar Suite / UAX (Unified Analyst Experience)

### ASM (Attack Surface Management)

### EDR (Endpoint Detection Response)

### Log Insights 

### SIEM (QRadar)

### SOAR

### X-Force Threat Intelligence and Expertise 

---
## Components

![[Pasted image 20250919125012.png]]

### 1. Event Collector (EC)

✅ **What it does**: Receives raw logs/events from external sources (e.g., syslog, CEF, LEEF). Performs initial normalization, coalescing, and forwards to Event Processors.  
✅ **Role in SOC**: First stop for incoming logs. Useful for troubleshooting ingestion issues (e.g., missing logs, log format issues).  
🧠 **Learn**: How to verify log arrival using tools like `tcpdump`, `logrun.pl`, and manage log source configurations.

### 2. Flow/QFlow Collector / Flow Processor

✅ **What it does**: Collects and processes network flow data (NetFlow, sFlow, QFlow, etc.). Helps detect lateral movement and unusual traffic.  
✅ **Role in SOC**: Enables flow-based detection, which is essential for network-based threat analysis and visibility.  
🧠 **Learn**: How to analyze flows in the **Network Activity** tab and correlate them with offenses.

### 3. Event Processor (EP)

✅ **What it does**: Parses, normalizes, and processes incoming events from Event Collectors. Applies correlation rules and stores data in the Ariel database.  
✅ **Role in SOC**: Crucial for event correlation and rule triggering. Issues with rule firing often originate here.  
🧠 **Learn**: How to monitor CRE processes (`ecs-ep`), and troubleshoot rule performance or dropped events.

### 4. Magistrate Processing Core (MPC)

✅ **What it does**: Central offense manager that creates, updates, and stores offenses. Only one Magistrate per QRadar deployment.  
✅ **Role in SOC**: Controls offense lifecycle; understanding it helps L2s troubleshoot offense creation, rule matches, or delays.  
🧠 **Learn**: How to review Magistrate logs and verify offense correlation logic.

### 5. Ariel Database

✅ **What it does**: Stores time-series event and flow data. Supports querying via the AQL (Ariel Query Language).  
✅ **Role in SOC**: Foundation of historical investigations, threat hunting, and rule validation.  
🧠 **Learn**: How to write effective AQL queries and understand Ariel’s data retention and indexing.

### 6. Data Nodes

✅ **What it does**: Add-on nodes that expand storage and enhance search performance. Used in larger deployments.  
✅ **Role in SOC**: Help speed up log search and support long-term investigations without slowing QRadar down.  
🧠 **Learn**: How to monitor data node health, understand rebalancing, and configure retention policies.

### 7. Custom Rules Engine (CRE)

✅ **What it does**: Runs correlation rules on event and flow data. Triggers alerts, emails, offenses, and other actions.  
✅ **Role in SOC**: Core of detection logic. L2s often tune or create new correlation rules.  
🧠 **Learn**: Rule creation, use of building blocks, reference sets, and understanding CRE performance.

### 8. User Interface / Console

✅ **What it does**: The web-based management interface for all QRadar components. Hosts dashboards, offense views, searches, admin tools, etc.  
✅ **Role in SOC**: Main interface for analysts and engineers. L2s need to know how to use advanced tabs and configure system behavior.  
🧠 **Learn**: Navigation of Log Activity, Offenses, Admin, and Reports. Also, learn how to deploy changes and manage apps.

### 9. PostgreSQL Database (on Console)

✅ **What it does**: Stores QRadar configuration, offense metadata, identities, assets, and reference set data.  
✅ **Role in SOC**: Supports offense search, system configuration, and app functionality.  
🧠 **Learn**: How QRadar separates event data (Ariel) from configuration/offense data (Postgres), and how to back it up.

### 10. Apps & Extensions

✅ **What it does**: QRadar supports apps like:
- QRadar Assistant
- MITRE ATT&CK Mapping
- Threat Intelligence App  
    ✅ **Role in SOC**: L2s can install/manage apps that improve detection, automate enrichment, or assist in investigations.  
    🧠 **Learn**: How to manage extensions from the Admin tab and use the IBM X-Force App Exchange.

---

