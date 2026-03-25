### **Status**

- Tracks incident progress:
    
    - `New`: Default state on creation.
        
    - `Active`: Currently under investigation.
        
    - `Closed`: Investigation complete.
        
- Upon closure, select a resolution:
    
    - **True Positive** → Confirmed malicious activity
        
    - **Benign Positive** → Expected but flagged behavior
        
    - **False Positive (Logic)** → Alert logic incorrect
        
    - **False Positive (Data)** → Alert triggered due to inaccurate data
        
    - **Undetermined** → Inconclusive

### **Severity**

- Initially set by:
    
    - Analytics rule
        
    - Security product (e.g., Defender)
        
- Can be manually adjusted based on further analysis.
    
- Severity levels:
    
    - `Informational`
        
    - `Low`
        
    - `Medium`
        
    - `High`

### Use the Investigation Graph

- Accessible via **"Investigate"** on the Incident page.
    
- Visual tool to:
    
    - Identify all **entities** involved
    - Map **relationships** and **attack paths**
    - Analyze **alert timelines** and **correlations**



















