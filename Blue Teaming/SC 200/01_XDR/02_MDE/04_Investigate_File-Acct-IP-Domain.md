# File

### 🔔 Alerts Tab
- Lists **alerts related to the file**
- Similar to the general Alerts queue
- *Excludes* device group details

### Sample Collection Policy

When sample collection is configured, then check the following registry value:

- Path: `HKLM\SOFTWARE\Policies\Microsoft\Windows Advanced Threat Protection`
- Name: **AllowSampleCollection**
- Type: `DWORD`
- Hexadecimal value:
	- Value = `0` – block sample collection
	- Value = `1` – allow sample collection
### File response actions

- Stop and Quarantine File (Microsoft Defender AV *required*)
	- Remove Quarantine: `“%ProgramFiles%\Windows Defender\MpCmdRun.exe” –Restore –Name EUS:Win32/CustomEnterpriseBlock –All`
- Add Indicator 
- Download file
- Action center


---
# Account

### 📍 Where to Find User Info

- **Dashboard** – _Users at Risk_ widget
    
- **Alert Queue**
    
- **Device Details Page**
    

> 🔗 Usernames are clickable and open the **User Account Details Page**


####  🔹User Details (Left Pane)

- Open incidents, active alerts
    
- SAM name, SID
    
- Logged-on devices (count)
    
- First seen / last seen
    
- Role & log-on types
    
- More data based on enabled integrations
    

#### 🔹 **Overview Tab**

- Incidents involving the user
    
- Devices the user has logged on to
    
- Logon events per device (expandable)
    

#### 🔹 **Alerts Tab**

- Alerts **associated with the user**
    
- Shows:
    
    - Last activity date
        
    - Alert description & severity
        
    - Device involved
        
    - Status & assignment
        

#### 🔹 **Observed in Organization**

- Devices the user logged onto (within a custom date range)
    
- Shows:
    
    - Most/least frequent user per device
        
    - Total users per device
        
    - Expand each row for device details
        
    - Clickable links to navigate to device pages

---
# IP

Identify communication between **internal devices** and **external IPs** (e.g., C2 servers) to assess breach scope and take action (e.g., quarantine devices/files).

### 🔎 How to Investigate

1. In the **Search bar**, select `IP`.
    
2. Enter the IP address and press `Enter`.
    
3. Review the following data:

### 📋 IP Address View Sections

#### 🌍 **IP Worldwide**

- ASN details
    
- Geolocation data
    

#### 🔁 **Reverse DNS Names**

- Domains resolving to the IP
    

#### 🚨 **Alerts Related to This IP**

- List of alerts tied to this IP
    

#### 🏢 **IP in Organization**

- Number of org devices communicating with the IP
    

#### 📈 **Prevalence**

- First/last seen timestamps
    
- Number of affected devices
    
- Filterable (default: 30 days)
    

#### ⏱ **Most Recent Observed Devices**

- Timeline view of related events and alerts

---
# Domain

Check if devices or servers in your network have communicated with a **known malicious domain**, to identify threats and their scope.

### 🔎 How to Investigate

1. In **Search bar**, select `URL`.
    
2. Enter the **domain** and press `Enter`.
    
3. Review search results (only available if domain has been observed in org traffic).
    

### 🧭 URL View Sections

#### 📄 **URL Details**

- Contacts
    
- Nameservers
    
- Global insights
    

#### 🚨 **Alerts Related to URL**

- Filtered list of alerts involving the domain
    
- Severity, status, investigation state, incident links
    

#### 🏢 **URL in Organization**

- Devices that communicated with the domain
    
- Prevalence stats (default: 30 days; range: 1–180 days)
    

#### 📊 **Incident Card**

- Bar chart showing active alerts (up to 180 days)
    

#### 📈 **Prevalence Card**

- View how common the URL is in the org
    

#### ⏱ **Most Recent Observed Devices**

- Timeline of observed events and alerts involving the URL
    

#### 🗂 **Observed in Organization Tab**

- Device, time, description of event
    
- Fully filterable by time and device