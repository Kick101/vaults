- The *Common Event Format connector* deploys a **Syslog Forwarder server** to bridge communication between appliances (Syslog sources: firewalls etc) and **Microsoft Sentinel**.
- Uses a **dedicated Linux VM** with the **Log Analytics agent for Linux** installed.

### Deployment Models

#### 1. Azure-hosted Linux VM

- On-prem Syslog sources -> securely send logs -> **Azure Linux VM**
- The Azure VM (with agent) -> forwards logs -> Microsoft Sentinel workspace

#### 2. On-prem or Other Cloud Linux VM

- Syslog sources -> securely send logs -> **on-prem/3rd-party Linux VM**
- The VM (with agent) -> forwards logs -> Microsoft Sentinel workspace

### Supported Syslog Daemon Versions

- **Syslog-ng**: versions **2.1 -> 3.22.1**
- **rsyslog**: version **v8**

### ✅ **Syslog Protocol Standards (RFCs)**

- **RFC 3164** (BSD Syslog)
- **RFC 5424** (IETF Syslog)
### ✅ **Other System Requirements**

- **Permissions**: Must have *sudo* (elevated) access on the Linux machine
- **Python**: Version **2.7 or 3.x** must be installed

---

### CEF Deployment Script

- Installs **Log Analytics agent for Linux** (also known as *OMS agent*)
    
    - OMS agent **Listens** for CEF messages on **TCP port 25226**
    - **Sends** messages securely over **TLS** to Microsoft Sentinel
    
- Configures **Syslog daemon (rsyslog or syslog-ng)**
    
    - **Listens** for Syslog messages from source solutions on **TCP port 514**
    - **Filters** for CEF-formatted messages only
    - **Forwards** filtered CEF logs *to the OMS agent* on **localhost:25226**

### How it works:

1. **Security device** (e.g., firewall, proxy) sends **CEF-formatted logs** via Syslog over UDP/TCP to Linux collector
    
2. Linux box receives logs via `rsyslog` or `syslog-ng`
    
3. The **agent on Linux** forwards logs to Microsoft Sentinel via Azure Monitor


### Avoid Event Duplication (CEF + Syslog)

If you're using the **same Linux machine** to forward both:

- **Syslog (plain)**
    
- **CEF-formatted messages**
    

Then to prevent duplication between `Syslog` and `CommonSecurityLog` tables in Sentinel:

- On **each source system** that sends logs in **CEF format**,  
    -> Edit the **Syslog configuration**  
    -> **Remove the facilities** (e.g., `local4`, `authpriv`, etc.) used for CEF message routing

This ensures that:

- CEF logs are forwarded via the CEF pipeline only
    
- Plain Syslog logs don't redundantly include those CEF events

---
