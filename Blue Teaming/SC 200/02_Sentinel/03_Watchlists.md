- Microsoft Sentinel provides a **table to store list data** that is accessible using **KQL**.
    
- The **Watchlists page** in Sentinel offers management options to maintain these lists.

- **Purpose**: Collect data from external sources for correlation with events in Microsoft Sentinel.
    
- **Usage**: Applied in searches, detection rules, threat hunting, and response playbooks.
    
- **Storage**: Saved in the Sentinel workspace as **name-value pairs**, cached for **low-latency, high-performance** querying.

### **Common Use Cases**

- **Threat Investigation & Incident Response**
    
    - Rapid import of CSV data (e.g., IPs, file hashes)
        
    - Use in joins/filters within:
        
        - Alert rules
            
        - Threat hunting queries
            
        - Workbooks, notebooks
            
        - General KQL queries
            
- **Business Context Integration**
    
    - Import user data such as:
        
        - Privileged access accounts
            
        - Terminated employees
            
    - Use to build:
        
        - **Allowlists** (trusted users/assets)
            
        - **Blocklists** (restricted users/assets)
            
- **Alert Fatigue Reduction**
    
    - Suppress alerts from known, authorized sources
        
    - Prevent benign or routine activities from generating noise
        
- **Event Data Enrichment**
    
    - Add contextual data via watchlist key-value pairs
        
    - Enhance correlation and visibility in detections

---
## Create a watchlist

![[Pasted image 20250801111830.png]]

- Navigate to:  
    **Microsoft Sentinel > Configuration > Watchlist**, then select **Add new**.
    
- **General Page**:
    
    - Enter the **Name**, **Description**, and **Alias** for the watchlist.
        
    - Click **Next**.
        
- **Source Page**:
    
    - Choose the **dataset type**.
        
    - **Upload the file** containing your list data.
        
    - Click **Next**.
        
    
    > **Note**: File size is limited to **3.8 MB**.
    
- **Review + Create**:
    
    - Verify all input details.
        
    - Select **Create**.
        
    - A notification confirms once the watchlist is successfully created.

### **Accessing Watchlist Data in KQL**

- Use the following function:

```sql
_GetWatchlist('watchlist_alias')
```

- Replace `'watchlist_alias'` with the alias specified during creation.
    
- Enables use of watchlist data in **queries, detection rules, and hunting queries**.

---
## Manage watchlists

#### **General Recommendation**

- **Avoid deleting and recreating watchlists.**
    
    - **Reason**: Log Analytics has a **5-minute ingestion SLA**.
        
    - If deleted and recreated within this window, **duplicate entries** may appear.
        
    - If duplicates persist beyond 5 minutes, **submit a support ticket**.
### **Edit a Watchlist Item**

1. In the **Azure portal**, go to **Microsoft Sentinel** and select the appropriate **workspace**.
    
2. Under **Configuration**, select **Watchlist**.
    
3. Choose the watchlist to edit.
    
4. In the details pane, select:
    
    - **Update watchlist > Edit watchlist items**
        

##### **To Edit an Existing Item:**

- Select the checkbox next to the item.
    
- Modify the fields as needed.
    
- Click **Save**.
    
- Confirm by selecting **Yes**.
    

##### **To Add a New Item:**

- Select **Add new**.
    
- Fill in the required fields in the **Add watchlist item** panel.
    
- Click **Add**.

### **Bulk Update a Watchlist**

- **Use Case**: Efficient when adding **multiple items**.
    
- **Functionality**:
    
    - **Appends** new items.
        
    - Performs **deduplication** where all column values match.
        
- **Limitation**:
    
    - Bulk update **does not remove** items missing from the uploaded file.
        
        - To delete items:
            
            - Remove them **manually**, or
                
            - **Delete and recreate** the watchlist if many items need removal.
                

##### **Bulk Update Steps**:

1. In **Azure portal**, go to **Microsoft Sentinel** > select workspace.
    
2. Navigate to **Configuration > Watchlist**.
    
3. Select the target watchlist.
    
4. In the details pane:
    
    - Click **Update watchlist > Bulk update**.
        
5. Under **Upload file**, drag and drop or browse to the CSV.
    
6. If an error occurs:
    
    - Fix the file.
        
    - Click **Reset**, and re-upload.
        
7. Select **Next: Review and update > Update**.
    

> **Note**: The uploaded file **must include the search key field** with **no blank values**.