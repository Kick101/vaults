sc2
#### (ISC)2 Code of Ethics
__Four Canons:__
1. Protect society and infrastructure
2. Act honorably. (Work with laws)
3. Serve principals diligently and competent. (responsible manner)
4. Advance the information security profession.
- _Failed to report violation is a violation_
- Procedure to submit violation:
	- Written and notarized affidavit using (ISC)2 form
	- It must include, name of the accused, nature of the violation, which canon breached, reason we are reporting and evidence.

__Standing to file a complaint:__
- Standing varies by canon involved.
- Anyone may file a complaint under canons 1 and 2.
- Canon 3 may be filed by employers or clients
- Canon 4 may be filed by professionals who are subscribed or licensed.

---
#### Authentication
__Authentication Factors:__
- Something you know, pin & password.
- Something you have, phone, credit card & keys.
- Something you are, biometrics; fingerprint, eye pattern, voice etc

__Multifactor Authentication:__
- Combining multiple authentication factors, like password and smart cards.
- Fingerprint and pin.
- _Password and security question is not multifactor authentication_
- Multifactor authentication means _different factors must be used_.

__Single Sign On(SSO):__
- Shares authenticated sessions across systems. (Google SSO)
- User logs on to the first SSO enabled system, then that login session persists across other systems until it expires.  

---
__Non-repudiation:__
- Physical, electronic, digital signatures and biometrics provide non-repudiation.

__Privacy Concerns:__
- Protecting our own data
- Education our users
- Protecting data collected by our organizations
- _Personally Identifiable Information_ - data that can tied back to a specific individual
- _Protected Health Information_ - Healthcare records that are regulated under the Health Insurance Portability and Accountability Act (HIPPA)
- Privacy programs are based upon a legal principle known as the _reasonable expectation of privacy_
- __Ex:__ Employee using organization computer should not have expectation of privacy, the employer has legal right to monitor your use of their systems and network.

---
#### Risk Management
The main responsibility of a cybersecurity professional is to identify, assess and manage risk to protect information and assets.

__Internal Risk:__
- Risks that arise within the organization
-  Improper processing of checks may provide opportunity for accounting department to commit fraud.
- This can be addressed by adding internal controls; like by adding two-person control to process a check, might reduce the risk.

__External Risk:__
- Risks that arise outside the organization
- Like attacker attempting ransomware attack
- Can be addressed by using multifactor authentication, social engineering awareness training

__Multi-party risks:__
- Risks that are shared among many different organizations
- __Ex:__ If SaaS compromised then, it poses risk to customers.

__Other risks:__
- Legacy systems risk
- Intellectual property theft risk 
- Software license compliance issues risk

##### Risk Assessment
Risk assessment is the process of _identifying and triaging_ the risks facing an organization _based upon the likelihood of their occurrence_ and their _expected impact_ on the organization.
- Risks are combination of _threat and vulnerability_. 
- There is _no risk if there is no threat or vulnerability_.
- _Ranking risks_ based upon their likelihood and impact.

__Qualitative Risk Assessment__
Uses subjective ratings to evaluate risk likelihood and impact.
- low, medium and high

__Quantitative Risk Assessment__
Uses objective numeric ratings to evaluate likelihood and impact.
- CVSS score. 

##### Risk Treatment
Analyzes and implements possible responses to control risks.
- Risk avoidance
- Risk transference (Insurance policy) risk may not be transferred completely.
- Risk mitigation (Reduce likelihood or impact)
- Risk acceptance 

_Risk profile_ is the set of risks and risk management strategies that organization faces.
_Inherent risk_ - initial risk before any controls.
_Residual risk_ - risk that is left after applying controls on inherent risk.
_Control risk_ - risk possessed by controls themselves like firewall failure
_Risk tolerance_ - level of risk an organization is willing to accept.

> The goal of risk management is to make sure that the combination of the residual risk and control risk is below the risk tolerance level. 

---
### Governance
- GDPR
- PCI - Compliance is enforced by banks
- DSS

__Policies:__
_AUP:_ What is acceptable to do; technology & usage; unauthorized
_Data handling Policies:_ types of sensitive information, how they should be handled
_Password policies:_ password expiration, length, requirements
_BYOD:_ covers the use of personal devices
_Privacy Policy:_  what info about users/employees the org retains about them
_Change management policy:_ covers the documentation, approval and rollback of tech changes

---
### Business Continuity Plan
A collection of activities  designed to keep a business running in the face of adversity.
- To keep _Continuity of Operations Planning(COOP)

__Defining the BCP Scope__
- What _business activities_ will the plan cover?
- What _systems_ will it cover?
- What _controls_ will it consider?

__Business Impact Assessment:__ _Identifies & prioritizes risks_
- Follows either quantitative or qualitative
-  Cloud BCP: collaboration b/w providers & customers
- Customer can  use redundancy in cloud as BCP
- Provider can implement physical operations

__Availability:__ Redundant
__Single Point  of Failure Analysis:__ Identify and remediate _spof_

__IT Contingency Scenarios__
- Sudden bankruptcy
- Insufficient storage or compute capacity
- Failure  of utility service
-  _Personnel succession planning_ - highly skilled team members, potential replacements 

__High Availability__
- Uses multiple systems to protect against service failure
- Operationally redundant systems

__Fault Tolerance__
- Makes a single system resilient against technical failures
- Load balancing

__Common points of failure__
- Power supply (Dual power supply, UPS, PDUs)
- Storage media (Redundant Array of Inexpensive Disks (_RAID_)) (_Fault tolerance_)
	- _mirroring_ - each disk has identical contents, two disk are written at once
	- _Striping_ - (RAID 5) 3 or more disks but it also includes additional info as parity blocks that are spread across the disks. In failure, system can regenerate disk contents using these blocks.
- Network Redundancy: multiple ISP, multiple interface cards & critical servers
-  _Using more than one NIC is NIC teaming_
-  _Multipath_
- Redundancy through diversity: if one type is failing
	- Different vendors
	- Different tech
	- Cryptography

---
### Disaster Recovery
__Initial Response Goals__
- Contain the damage
- Recover normal operations
After the immediate danger to the org clears, the disaster recovery team shifts from _immediate response mode to assessment mode_. 

__Recovery Time Objective (RTO)__ is the amount of time to restore service.

__Recovery Point Objective (RPO)__ is amount of data to recover.

__Recovery Service Level (RSL)__ is the percentage of service to restore.

##### Backups


---
### Incidence Response
Incidence response plan that outlines the polices, procedures and guidelines that the organization will follow when an incidence take place.

##### Incident response plan elements
- Statement of purpose (what incident)
- Strategies and goals for incident response
- Approach to incident response (who's responsible & their authority)
- Communication with other groups
- Senior management

##### Building and staffing Incidence response team
- Management
- Information security
- Subject matter experts (Developers, system engineers etc)
- Legal counsel
- Public affairs
- Human resources
- Physical security
Use incident response service providers if necessary. 

##### Incident Data sources
- IDS/IPS
- Firewall
- Authentication system
- Integrity monitors
- Vulnerability scanners
- System event logs
- Netflow records
- Anti-malware packages

__Security Incident and Event Management(SIEM)__
Security solution that collects information from diverse sources, analyzes it for signs for security incidents and retains it for later use 

> The highest priority of a _first responder_ must be _containing damage through isolation_.

---
### Security Controls
Reduce the likelihood or impact of a risk and help identify issues.

__Defense in Depth:__
It uses overlapping security controls.

#### Grouping Controls
__By their Control purpose__
- _Preventive_ - stop from occurring, firewall
- _Detective_ - require further investigation, IDS
- _Corrective_ -  remediate issues that have occurred, backup.

__By their Control mechanism__
- _Technical/Logical_ - technology, firewall, IDS/IPS
- _Administrative_ - user access reviews, log monitoring, awareness training
- _Physical_ - locks, cameras

#### Physical access controls
- Gates
- Alarm Systems
- Bollards: stop vehicles
- _Crime Prevention Through Environmental Design_ (CPTED)
	- Natural Surveillance: windows, lighting, 
	- Natural Access Control: gates
	- Natural Territory Reinforcement
- _Visitor Procedures_
	- Describe allowable visit purposes
	- Explain visit approval authority
	- Describe requirements for un-escorted access
	- Explain role of visitor escorts
- _Logs_ should be maintained
- Visitor _Identification badge_ is a must
- _Robot sentries:_ robot security patrols
- _Two person Integrity:_ Two people required to access a area
- _Two person Control:_ Two people must approve sensitive action

#### Logical/Technical Access Controls
- _Job Rotation:_ moves employees through different positions/locations
- _Mandatory Vacation policies:_ Frauds may come to light
- _Account life-cycle:_ account and credential life-cycle should be manages

##### Continuous Account Monitoring
 - Watch for suspicious activity
 - Alter administrators to anomalies
 - _Access policy violations:_
	 - impossible travel time logins
	 - Unusual network location logins
	 - Unusual time-of-day logins
	 - Deviations from normal behaviour
	 - Deviations in volume of data transferred
 - _Geotagging:_ adds user location info to logs
 - _Geofencing:_ alerts when a device leaves defines boundaries

__User Account Audits:__
- Pull permission list
- Review with managers
- Make adjustments





---
### Networking
__Rj-45__
- 8 copper pins
- Ethernet

__Rj -11__
- 6 pins
- landlines

__NFC__
- Very near. No more than 2 inches
- Payments, building access control systems

#### TCP/IP
__Internet Protocol(IP):__
- Routes information across networks
- Provides an addressing scheme
- Delivers packets across network 
- _Network Layer_

__Transmission Control Protocol:__ 
- Connection oriented: E-mail, web traffic
- _3-way handshake flags:_ 
	- SYN: Opens a connection
	-  FIN: Closes a connection
	- ACK: acknowledges a SYN or FIN packet
	- SYN - SYN/ACK - ACK

__User Datagram Protocol:__
- Connectionless: voice, video  
- Doesn't send acknowledgements

__Open Systems Interconnection Model:__
1. Physical Layer: Wires, radios and fiber optics
2. Data Link Layer: Data transfers b/w two nodes
3.  Network Layer: Internet Protocol
4. Transport Layer:  TCP and UDP
5. Session Layer: Exchanges  b/w systems
6. Presentation Layer: Data translation & encryption
7. Application Layer: User programs

 ![[Pasted image 20230206121640.png | 500]]

##### Addressing Scheme
__IPv4:__
- 32bits
- network address & host address

__IPv6:__
- 128bits, 8 groups of 4 hexdigits
- hexadecimal

__Network Address Translation:__
- Translates private IP to public IP; router.
 
__Configuring IP:__
- Static, manually, used for servers
- Dynamic Host Configuration protocol, end-user devices

##### Ports
- Each port represents an application.
- 16 bit binary number; 2^16 - 65,536; 0 - 65,535
- 0 - 1,023: well known ports
- 1,024 - 49,151: registered ports
- 49,152 - 65,535: dynamic ports

##### Wireless Networks
- Open Networks - No password
- Service Set Identifier (SSID)
- Pre Shared Key(PKI) - Password based authentication
- Enterprise Authentication -  individual username & passwords are used to authenticate
- Captive Portal - webpage authentication

__Wireless Encryption:__
- Wired Equivalent Privacy (WEP): 
- WiFi Protected Access (WPA): Temporal Key Integrity Protocol (TKIP) changes encryption keys with temporal key for each packet. Advanced Encryption Standard (AES) is also used.
- WiFi Protected Access v2 (WPA2): (CCMP) Counter Mode Cipher Block Chaining Message Authentication Code Protocol based on Advanced Encryption Standard.
- WiFi Protected Access v3 (WPA3): CCMP supported and Simultaneous Authentication of Equals (SAE) is a secure Key exchange protocol based upon the Diffie-Hellman technique.
- WPA2 Enterprise: RADIUS server

---
### Network Security
#### Malware
__Propagation:__ Techniques malware uses to spread.
__Payload:__ Malicious actions taken by malware

##### Virus
__Propagation:__ User actions; email attachments

##### Worms
__Propagation:__ Spread by their own by exploiting vulnerabilities
__Avoid:__ Update with latest patches

##### Trojan Horse
Application control protects against this.
- General application with payload inside.

#### Botnets
Collection of zombie computers used for malicious purposes
- Renting out computing power
- Delivering spam
- Engaging in DDoS attacks
- Mining Bitcoin
- Waging brute force attacks
- Command and control network relay orders
	- Internet Relay Chat (IRC)
	- Twitter
	- Peer-to-peer within the botnet
- Communication must be indirect and redundant

#### Eavesdropping
- Network device tapping
- DNS poisoning
- ARP poisoning
- SSL Stripping

#### Replay Attacks
__Avoid:__ Session tokens, Timestamps

 #### Implementation Flaws
 ##### Fault Injection Attacks
 - Compromise the integrity of a cryptographic device by external fault
	 - High-voltage electricity
	 - High/Low temperature
- Device may not encrypt data properly and disclose un-encrypted data etc

 ##### Side Channel Attacks

##### Timing Attacks

---
### Threat Identification and Prevention
#### IDS/IPS
- __Signature Based detection__ - rule based detection
	- Will fail to identify new signatures
	- Reduce false positive rates
- __Anomaly/Behaviour/Heuristic Detection System__
	- Builds models of "normal" activity
	- High false positive rates
- __In-Band/Inline Deployment__
	- Device sites in the path of network communications
	- Single out of failure; may stop all traffic
- __Out-of-band/passive Deployment__
	- Device sites outside the path of network communications
	- Device connects to a SPAN port on a switch
	- It cannot stop the initial attack

#### Anti-malware
- Signature based
- Behavior/Anomaly based detection
	- Endpoint Detection and Response (EDR)
	- Analyze memory, registry entries, network, other behaviours
	- Sand-boxing: runs a suspicious software in a sandbox and watches it's behavour

#### Scanners
__Port scanner:__ Probes for open ports; 0-65535; nmap
__Vulnerability Scanner:__ Probes for known vulnerabilities
__Application Scanner:__ Tests for security flaws in applications

#### Network Security Infrastructure
##### Environmental Threats
- Air Temperature: 64.4F - 80.6F
- Humidity: low - static, high - water condensation; 41.9F- 50.0F
- _HVAC:_ Heating, Ventilation, Air Conditioning System. 
- Fire - wet/dry pipe systems, Chemical systems, 
- _MoU_ (Memorandum of understanding) - Type of agreement between two or more parties that outlines the terms and details of an understanding, including each party's obligations and responsibilities.
- _SLA_ (Service Level Agreement) - A contract between a service provider and a customer that outlines the level of service that the provider will deliver to the customer.

##### Network Border Firewall
- Connect to 3 different security zones
	- Internet (Untrusted network)
	- DMZ (Demilitarized Zone) - Email, Web servers
	- Internal Network (Intranet)

__Zero Trust:__ Access to resources is only granted after a thorough verification process.

__Extranet:__ Intranet segments accessed by external parties (VPN).

__Honeynet:__ Decoy networks designed to attract attackers to study their behaviour

__Ad Hoc Network:__ Temporary networks that may bypass security controls

__East-West Traffic:__ Network traffic b/w systems located in the data center

__North-South Traffic:__ Systems in the data center and internet

##### Network Hardware
__Conduits:__ Type of protective casing or tubing used to cover and protect electrical wiring and cables

__VLAN:__ 
- Separate systems on a network into logical groups.
- Enable _VLAN Trunking_: To allow switches in different locations to carry the same VLAN
- Configure each switch port to connect to the appropriate VLAN
- Micro segments: uses very small network segments to one.


__Firewalls__
- _Stateless firewall:_ Evaluate each connection independently
- _Stateful inspection:_ When outbound is allowed, returning inbound is also allowed
- _Firewall Rule Contents:_
	- Source system Address
	- Destination system Address
	- Port and Protocol
	- Action (Allow or Deny)
- Implicit Deny
- _NGFW:_ 
	- Application level inspection
	- IPS
	- Threat Intelligence
	- Unified Threat Management(UTM): URL filtering, Email scanning, Data Loss Prevention
	- user identity, application nature, time
- Firewall also does NAT; NAT gateway
- Hardware appliance and virtual appliance

##### VPN
- Site-to-site
- Remote access (often rely on SSL/TLS than IPSec)
- __VPN Endpoints:__
	- Firewalls
	- Routers
	- Servers
	- VPN Concentrators
- _VPN Protocols:_ PPTP, L2TP/IPSec, OpenVPN, IKEv2
- Layer 2 Tunneling Protocol (L2TP) (Data link Layer), over IPSec connection
- _HTML5 VPN_ - website VPN like a proxy
- Web browser based VPN
- Full Tunnel VPN (Recommended) and Split Tunnel VPN
- Always On VPN - boot-up

##### IPsec suite
**Authentication Header (AH):** The AH protocol ensures that data packets are from a trusted source and that the data has not been tampered. These headers do not provide any encryption

**Encapsulating Security Protocol (ESP):** ESP encrypts the IP header and the payload for each packet — unless transport mode is used, in which case it only encrypts the payload. ESP adds its own header and a trailer to each data packet.

**Security Association (SA):** SA refers to a number of protocols used for negotiating encryption keys and algorithms. One of the most common SA protocols is Internet Key Exchange (IKE).

##### Network Access Control
NAC interceots network traffic coming from unknown devices and verifies that the system and user are authorized before allowing.
- NAC uses _802.1x authentication_ protocol
	- Device that wants to authenticate uses _NAC Supplicant_ software
	- _Authenticator_ - switch/wireless controller
	- _Authentication server_ - performs authentication
- _RADIUS:_ Remote Authentication Dial-In User Service protocol
	- authentication, authorization, and accounting (AAA) of network users
	- RADIUS Access-Request
	- RADIUS Access-Reject
	- RADIUS Access-Accept
- _Role-based:_ use of VLANs
- _Posture Checking:_ security policy; anti-virus is running
	- Agent-based checking 
	- Agentless checking: scans device externally
- Inband
- Out-of-band

---
### Cloud Computing
Delivering computing resources to a remote customer over a network

- SaaS - wix, wordpress, microsoft 365, shopify
	- _Vendor Responsibility:_ Hardware, Data center, App, OS
	- _Customer Responsibility:_ Data
- IaaS - AWS, Azure, Google compute 
	- _Vendor Responsibility:_ Hardware, Data center 
	- _Customer Responsibility:_ Data, App, OS
- PaaS - Heroku
	- _Vendor Responsibility:_ OS, Hardware, Data center 
	- _Customer Responsibility:_ Data, App


__Cloud Deployment Models:__
- Private - flexibility, scalability, agility and cost effectiveness
- Public - Multi-tenancy infrastructure 
	- Shared responsibility model
- Hybrid - Both; if data is sensitive
- Multi - Combines resources from more than one cloud vendor
- Community Cloud - shared among orgs

__Managed Service Providers:__ Offer tech service to customers

__Managed Security Service Providers/Security as a service:__ Vendors that provide security services
- Complete
- Specific task

__Cloud Access Security Brokers (CASB):__ security solution that sits between an organization's users and its cloud services, monitoring and controlling access to the cloud.
- _Network Based:_
	- Analyzes traffic between users & cloud services and enforcing security policies
- _API Based:_
	- CASB solution that integrates with cloud services using APIs

##### Vendor Management Life-cycle
- Vendor Selection (Formal Request for Proposal or informal)
- On-boarding (Verify contract details) 
- Monitoring (Customer monitors vendors)
- Off-boarding (Vendor destroys confidential information, disposal of relation)

#### Agreements
__Non-disclosure Agreement (NDA):__ Protects the confidentiality of information.

__Service-Level Requirements(SLR):__ Specifies requirements that a customer has from vendor
- System Response time
- Availability of service
- Data preservation

__Service-Level Agreement(SLA):__ Contract b/w vendor and customer; penalties on vendor

__Memorandum of understanding(MoU):__ Used when legal dispute is unlikely; but sill use as a formal documentation

__Business Partnership Agreement(BPA):__ Two companies doing business & division of profits

__Interconnection security Agreement(ISA):__ Details of how orgs interconnect their networks, systems and data
- Encryption standard
- Transfer protocols

__Master services agreement(MSA):__ Many projects with a vendor

__Statement of work(SOW):__ Specific terms of an engagement; governed by the terms of MSA

---
### Security Operations
#### Data Handling
Data itself goes through its own life cycle as users create, use, share and modify it.

##### Data Life cycle
- Create
- Store
- Use
- Share
- Modify
- Archive
- Destroy

##### Data handling Practices
__Classification:__ Process of recognizing the organizational impacts if the information suffers any security compromises related to its characteristics of confidentiality, integrity and availability. It _dictate rules and restrictions_ about how that information can be used, stored or shared with others.

 __Labeling:__ A simple way of _assigning a level of sensitivity_ to a data asset, such that the higher the level, the greater the presumed harm to the organization.
- _Highly restricted_: Could possibly put the organization’s future existence at risk.
-   _Moderately restricted_: Could lead to loss of temporary competitive advantage, loss of revenue or disruption of planned investments or activities.
-   _Low sensitivity_ (or "internal use only"): Could cause minor disruptions, delays or impacts.
-   _Unrestricted public data_: Already published, no harm can come from further dissemination or disclosure.

 __Retention:__ 
 -   Data and information should be kept only for _as long as it serves a purpose_, defined by industry standards, laws, and regulations.
-   Organizations should have their own data retention policies, apply to both hard copies and electronic data, and _regularly review retained records_ to reduce storage and ensure necessary information is preserved.
-   Security professionals should _maintain accurate inventory_ and ensure data destruction when retention limit is reached.
-   _Retention policies should guarantee:_
    -   Understanding of retention requirements for different types of data throughout the organization.
    -   Appropriate documentation of retention requirements for each type of information.
    -   Systems, processes, and individuals retain information in accordance with the required schedule but no longer.
-   Applying the _longest retention period to all types of information is a common mistake_ that wastes storage and increases risk of data exposure and violation of external requirements and should be avoided.

 __Destruction:__
-   Data that is left on media after deletion is known as _remanence_.
-   _Clearing the device_ involves writing multiple patterns of random values throughout all storage media, called _"overwriting" or "zeroizing"_. It has the risk that a _missed blocks may contain recoverable, sensitive information_ after the process is completed.
-   _Purging the device_ Some magnetic disk storage technologies, can still have residual _"ghosts" of data_ on their surfaces even after being overwritten multiple times. _Degaussing:_ can be used, sometimes it may also not be sufficient.
-   _Physical destruction_ of the device or system is the ultimate remedy to data remanence.
-   In many routine operational environments, security considerations may accept that clearing a system is sufficient.
-   When systems elements are to be removed and replaced, purging or destruction may be required to protect sensitive information from being compromised by an attacker.

#### Encryption 
Encryption is the process of _converting plaintext (readable text) into ciphertext (unreadable text) using a secret key_, to protect it from unauthorized access.
Encryption ensures two important security principles: confidentiality(encryption) and integrity (hashing).

__Symmetric Encryption__: 
- Symmetric encryption is a method of _encrypting and decrypting data using a shared secret key_.
- Distribution of the key poses a challenge as it cannot be sent through the same channel as the encrypted message without risking the key being intercepted by a man-in-the-middle (MITM) attack. To mitigate this, the _key is distributed through an out-of-band method_, such as courier, fax, or phone, to ensure secure key delivery.
- Separate key is required for each person or group to communicate with another. However, this raises the _challenge of scalability_ as it is difficult to manage as the number of users increases. For example, an organization with 1,000 employees would need to manage 499,500 keys if all employees wanted to communicate with each other.
- _Primary uses of symmetric algorithms:_
	-   Encrypting bulk data (backups, hard drives, portable media)
	-   Encrypting messages traversing communications channels (IPsec, TLS)
	-   Streaming large-scale, time-sensitive data (audio/video materials, gaming, etc.)
_Examples_
- Any substitution cipher  
- AES (Advanced Encryption Standard)
-   DES (Data Encryption Standard)
-   3DES (Triple DES)
-   Blowfish
-   Twofish
-   RC4 (Ron's Code 4)

- _Other names for symmetric algorithms:_
	-   Same key
	-   Single key
	-   Shared key
	-   Secret key
	-   Session key

__Asymmetric Encryption:__
- Asymmetric encryption uses _different keys to encrypt and decrypt messages, solving key distribution and scalability issues_ but being _slower and less practical for large amounts of data_.

__Hashing:__
- 

#### Logging and monitoring security events
Logging is the process of _capturing and recording information about events_ that occur within a computer system or network.
- Logged data include but not limited to: 
	-   user IDs
	-   system activities
	-   dates/times of key events (e.g., logon and logoff)
	-   device and location identity
	-   successful and rejected system and resource access attempts
	-   system configuration changes and system protection activation and deactivation events
- Log reviews are essential for identifying security incidents, policy violations, and operational problems in a timely manner.
- __Ingress monitoring__: monitoring _incoming network traffic_ to detect and prevent unauthorized access
	- Tools:
		- Firewalls
		- Gateways
		- Remote authentication servers
		- IDS/IPS tools
		- SIEM solutions
		- Anti-malware solutions
- __Egress monitoring:__ monitoring _outgoing network traffic_ to detect and prevent data exfiltration or malicious activity. 
	- The _Data Loss Prevention_ solution is used to inspect all forms of data leaving the organization, including:
		- Email (content and attachments)
		- Copy to portable media
		- File Transfer Protocol (FTP)
		- Posting to web pages/websites
		- Applications/application programming interfaces (APIs)

#### Configuration management 
It ensures that systems are configured similarly, configurations are known and documented.
It consists of identification, baseline, change control, and verification and audit.
- __baselines:__ ensures that systems are deployed with a common baseline or starting point, and imaging is a common baselining method.
- __updates:__
- __patches:__