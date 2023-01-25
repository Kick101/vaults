__Learning Objectives:__
- Explain concepts of security operations
- Discuss data handling best practices
- Identify key concepts of logging and monitoring
- Summarize the different types of encryption and their common uses
- Describe the concepts of configuration management
- Explain the application of common security policies
- Discuss the importance of security awareness training
- Practice the terminology of and review the concepts of network operations 

---
### Understand data security
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
---
### Understand system hardening
#### Configuration management 
It ensures that systems are configured similarly, configurations are known and documented.
It consists of identification, baseline, change control, and verification and audit.
- __baselines:__ ensures that systems are deployed with a common baseline or starting point, and imaging is a common baselining method.
- __updates:__
- __patches:__

---
### Understand best practice security policies

---
### Understand Security Awareness Training