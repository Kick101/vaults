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
### Security Controls
Reduce the likelihood or impact of a risk and help identify issues.

__Defense in Depth:__
It uses overlapping security controls.

__Grouping Controls:__
By their _Control purpose_:
- Preventive - stop from occurring, firewall
- Detective - require further investigation, IDS
- Corrective -  remediate issues that have occurred, backup.

By their _Control mechanism_:
- Technical/Logical - technology, firewall, IDS/IPS
- Administrative - user access reviews, log monitoring, awareness training
- Physical - locks, cameras

##### physical access controls


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



