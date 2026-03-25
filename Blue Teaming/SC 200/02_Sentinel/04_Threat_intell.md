**Sources** of Cyber Threat Intelligence (*CTI*):

- Open-source intelligence (OSINT) feeds
    
- Threat intelligence-sharing communities
    
- Commercial/paid intelligence providers
    
- Internal security investigations

**Types of CTI**:

- **Strategic/Operational**: Reports on adversary motivations, tactics, infrastructure, and behaviors
    
- **Tactical**: Specific indicators (IP addresses, domains, file hashes, URLs) associated with known threats (e.g., phishing, botnets, malware)

### **CTI in Microsoft Sentinel**
    
- **Threat Indicators** or **Indicators of Compromise (IoCs)**
	
- Commonly ingested IoC types:
	
	- IP addresses
		
	- Domains/URLs
		
	- File hashes
            
- **Use Case**: Classified as **Tactical Threat Intelligence**—used in real-time by security products to detect and mitigate threats

---
## Integrating Threat Intelligence into Microsoft Sentinel

1. **Import Threat Intelligence Data**
    
    - Use **Data connectors** (e.g., TAXII, Microsoft Defender Threat Intelligence, custom APIs)
        
    - Enables ingestion from external TI platforms
        
2. **Manage & Query TI Data**
    
    - View imported indicators via:
        
        - **Log Analytics** (`ThreatIntelligenceIndicator` table)
            
        - **Threat Intelligence blade** in Sentinel
            
3. **Generate Alerts Using Analytics Rules**
    
    - Utilize **built-in analytics rule templates** to correlate TI with environment data
        
    - Automatically generate **alerts and incidents**
        
4. **Visualize Threat Intelligence**
    
    - Use the **Threat Intelligence workbook** to:
        
        - Monitor indicator volume and types
            
        - View trending IoCs
            
        - Assess TI coverage and usage
            
5. **Perform Threat Hunting**
    
    - Join threat indicators with logs to:
        
        - Identify signs of compromise
            
        - Investigate suspicious behavior proactively

---

## New Threat Intelligence Tables

Microsoft Sentinel has introduced **two new tables** to support the **STIX indicator and object schemas**:

- **`ThreatIntelIndicators`**
    
    - Replaces `ThreatIntelligenceIndicator`
        
    - Supports structured ingestion of IoCs (e.g., IPs, domains, file hashes) in STIX format
        
- **`ThreatIntelObjects`**
    
    - Designed for broader CTI object support, including TTPs, threat actors, campaigns, etc.