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
#### Security Controls
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

