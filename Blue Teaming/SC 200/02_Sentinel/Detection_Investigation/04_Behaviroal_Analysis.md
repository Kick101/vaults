 Builds **behavioral profiles** of entities (users, hosts, IPs, applications) using:

- Time-based analysis.
    
- Peer group comparison.
    
- Machine learning techniques.


**Identifies:**

- Asset sensitivity.
- Peer groups.
- **Blast radius** (impact of asset compromise).

### Security-driven analytics

Microsoft adopted Gartner’s paradigm for *UEBA* solutions, Microsoft Sentinel provides an "outside-in" approach, based on three frames of reference:

**1. Use cases:** 
Microsoft Sentinel prioritizes for relevant attack vectors and scenarios based on security research aligned with the *MITRE ATT&CK framework* of tactics, techniques, and subtechniques. The prioritization *identifies various entities as victims, perpetrators, or pivot points in the kill chain*. Microsoft Sentinel focuses specifically on the most valuable logs each data source can provide.

**2. Data Sources:**
While first and foremost supporting Azure data sources, Microsoft Sentinel thoughtfully selects third-party data sources to provide data that matches our threat scenarios.

**3. Analytics:**
Microsoft Sentinel uses *machine learning* (ML) algorithms, and identifies anomalous activities that presents evidence clearly and concisely in the form of contextual enrichments. 

![[Pasted image 20250808140424.png]]

### Scoring

- Every activity receives an *Investigation Priority Score* (scale: 0–10).
    
- Scoring is based on:
    
    - Likelihood of a specific user performing an activity.
        
    - Behavior comparison with user’s peers.
        
- **Higher scores** indicate **more abnormal** and **suspicious** activities.

---

You should verify that all of your alert providers properly identify the entities in the alerts they produce. Additionally, *synchronizing user account entities with Microsoft Entra ID* may create a unifying directory, which will be able to merge user account entities.

### Entity Insights

insights include *data regarding sign-ins, Group Additions, Anomalous Events and advanced ML algorithms* to detect anomalous behavior. The insights are based on the following data types:

- Syslog
    
- SecurityEvent
    
- Audit Logs
    
- Sign-in Logs
    
- Office Activity
    
- BehaviorAnalytics (UEBA)

**Entity pages can be accessed from** 
- incident management, 
- the investigation graph, 
- bookmarks, or 
- directly from the entity search page under **Entity behavior analytics** in the Microsoft Sentinel main menu.

The *Incident Investigation Graph* includes an option for *Insights*. Insights display information from the *Entity behavior data*.

---
## Anomaly detection analytical rule templates

While anomalies don't necessarily indicate malicious or even suspicious behavior by themselves, they can be used to improve detections, investigations, and threat hunting:

- Strengthen detection when multiple anomalies align with the kill chain.
    
- Enhance investigations by indicating new paths or confirming impact.
    
- Support **proactive threat hunting** through contextual clues.

### **Key Benefits**

- Anomalies can be powerful but are *often noisy and require heavy tuning*.
    
- Microsoft Sentinel anomaly templates:
    
    - *Pre-tuned by Microsoft*’s data science team for **out-of-the-box value**.
    - Configurable without **machine learning expertise**.        
    - Fine-tuning possible via **Analytics Rule UI**.

- Performance comparison between **original** and **modified** thresholds/parameters is built into the interface.
- Tuning process supports a **testing (flighting) phase** before production promotion.
- **One-click promotion** to production once tuning meets performance objectives.    
- Developed using **thousands of data sources** and **millions of events**.   
- Generated anomalies appear in the *Anomalies table* in the Logs section.

### Assessing Anomaly Quality

1. **Go to:** Microsoft Sentinel → Analytics → Active rules tab.
    
2. **Filter for:** Anomaly rules.
    
3. **Copy Rule Name** from details pane.
    
4. **Go to:** Logs > Tables tab (close Query gallery if open).
    
5. **Set Time Range:** Last 24 hours.
    
6. **Run KQL Query:**


```sql
Anomalies
| where AnomalyTemplateName contains "Rule_Name"
```

7. **Review Results:**

- Expand *AnomalyReasons* field for firing reason.
- Too many anomalies often indicate **threshold too low**.

### Tuning Anomaly Rules

- Original *active rules cannot be edited* directly.
    
- **Must duplicate** an active rule to customize.
    
- Original rule continues running until disabled or deleted.
    
- **Only one customized copy** per original rule allowed.
    
- Duplicate rules are **disabled by default**.

---

