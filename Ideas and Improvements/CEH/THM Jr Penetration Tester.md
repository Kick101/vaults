### Offensive Security
-   __Penetration Tester__ - Responsible for _testing technology_ products for finding exploitable security vulnerabilities.
- __Red Teamer__ - Plays the _role of an adversary_, attacking an organization and providing feedback from an enemy's perspective.
- __Security Engineer__ - _Design, monitor, and maintain_ security controls, networks, and systems to help prevent cyber-attacks.
---
### Defensive Security
#### Security Operations Center (SoC)
_A team of cyber security professionals that monitors the network and its systems to detect malicious cyber security events_.
- __Vulnerabilities__: Whenever a system vulnerability is discovered, it is essential to fix it by installing a proper update or patch. When a fix is not available, the necessary measures should be taken to prevent an attacker from exploiting it.
-   __Policy violations__: Set of rules required for the protection of the network and systems. For example, it might be a policy violation if users start uploading confidential company data to an online storage service.
-   __Unauthorized activity__: Consider the case where a user’s login name and password are stolen, and the attacker uses them to log into the network. A SOC needs to detect such an event and block it as soon as possible before further damage is done.
-   __Network intrusions__:  An intrusion can occur when a user clicks on a malicious link or when an attacker exploits a public server. We must detect it as soon as possible to prevent further damage.

#### Threat Intelligence
- _Threat intelligence aims to gather information to help the company better prepare against potential adversaries._
- _Intelligence_ refers to information you gather about actual and potential enemies. 
- _Threat_ is any action that can disrupt or adversely affect a system. 

#### Digital Forensics and Incident Response (DFIR)
##### Digital Forensics
Forensics is the application of science to investigate crimes and establish facts. New branch of forensics was born to investigate cyberspace related crimes: computer forensics, which later evolved into, _digital forensics_.
-   __File System__: Analyzing a digital forensics image (low-level copy) of a system’s storage reveals much information, such as installed programs, created files, partially overwritten files, and deleted files.
-   __System memory__: If the attacker is running their malicious program in memory without saving it to the disk, taking a forensic image (low-level copy) of the system memory is the best way to analyze its contents and learn about the attack.
-   __System logs__: Each client and server computer maintains different log files about what is happening. Log files provide plenty of information about what happened on a system. Some traces will be left even if the attacker tries to clear their traces.
-   __Network logs__: Logs of the network packets that have traversed a network would help answer more questions about whether an attack is occurring and what it entails.

##### Incident Response
_An incident usually refers to a data breach or cyber attack; however, it can be a misconfiguration, an intrusion attempt, or a policy violation._
- Incident response _specifies the methodology that should be followed to handle_. 
- The aim is to _reduce damage and recover in the shortest time possible_. Ideally, you would develop a plan ready for incident response.

Four major phases of the incident response process:
1.  __Preparation__: This requires a team trained and ready to handle incidents. Ideally, various measures are put in place to prevent incidents from happening in the first place.
2.  __Detection and Analysis__: The team has the necessary resources to detect any incident; moreover, it is essential to further analyze any detected incident to learn about its severity.
3.  __Containment, Eradication, and Recovery__: Once an incident is detected, it is crucial to stop it from affecting other systems, eliminate it, and recover the affected systems. For instance, when we notice that a system is infected with a computer virus, we would like to stop (contain) the virus from spreading to other systems, clean (eradicate) the virus, and ensure proper system recovery.
4.  __Post-Incident Activity__: After successful recovery, a report is produced, and the learned lesson is shared to prevent similar future incidents.

##### Malware Analysis
Malware stands for malicious software. Malware includes many types, such as:
-   __Virus__ is a piece of code that attaches itself to a program. It is designed to spread from one computer to another; moreover, it works by altering, overwriting, and deleting files once it infects a computer. The result ranges from the computer becoming slow to unusable.
-   __Trojan Horse__ is a program that shows one desirable function but hides a malicious function underneath. For example, a victim might download a video player from a shady website that gives the attacker complete control over their system.
-   __Ransomware__ is a malicious program that encrypts the user’s files. Encryption makes the files unreadable without knowing the encryption password. The attacker offers the user the encryption password if the user is willing to pay a “ransom.”

Malware analysis aims to learn about such malicious programs using various means:
1.  __Static analysis__ works by inspecting the malicious program without running it. Usually, this requires solid knowledge of assembly language.
2.  __Dynamic analysis__ works by running the malware in a controlled environment and monitoring its activities. It lets you observe how the malware behaves when running.
---
### Pentesting Fundamentals
**Rules of Engagement (ROE)**
The ROE is a document that is created at the initial stages of a penetration testing engagement. This document consists of three main sections.

|section|description|
|-|-|
|Permission|This section of the document gives explicit permission for the engagement to be carried out. This permission is essential to legally protect individuals and organisations for the activities they carry out.|
|Test Scope|This section of the document will annotate specific targets to which the engagement should apply. For example, the penetration test may only apply to certain servers or applications but not the entire network.
|Rules|The rules section will define exactly the techniques that are permitted during the engagement. For example, the rules may specifically state that techniques such as phishing attacks are prohibited, but MITM (Man-in-the-Middle) attacks are okay.|
#### Penetration Testing Methodologies
##### General Methodology
- Information Gathering
- Enumeration/Scanning
- Exploitation
- Privilege Escalation
- Post-Escalation

##### Open Source Testing Methodology Manual (OSSTMM)
- provides testing strategies for systems, software, applications, communications & the human aspect of cybersecurity.
- The methodology focuses primarily on how these systems, applications communicate, so it includes a methodology for:
	1.  _Telecommunications_ (phones, VoIP, etc.)
	2.  _Wired Networks_
	3.  _Wireless communications_

##### Open Web Application Security Project (OWASP)
- Used solely to test the security of web applications and services.

##### NIST Cybersecurity Framework 1.1
- Framework used to improve an organisations cybersecurity standards and manage the risk of cyber threats

##### Cyber Assessment Framework (NCSC CAF
- Extensive framework of 14 principles used to assess the risk of various cyber threats and an organisation's defences against these.
- The framework applies to organisations considered to perform "_vitally important services and activities_" such as critical infrastructure, _banking_, and the likes. The framework mainly focuses on and assesses the following topics:
	-   Data security
	-   System security
	-   Identity and access control
	-   Resiliency
	-   Monitoring
	-   Response and recovery planning
---
### Principles of Security
#### CIA Traid
- Confidentiality
- Integrity
- Availability

#### Principles of Privileges
The levels of access given to individuals are determined on two primary factors:
-   The individual's role/function within the organisation
-   The sensitivity of the information being stored on the system

__PIM__ - Privileged Identity Management. Used to translate a user's role within an organisation into an access role on a system
__PAM__ - Privilege Access Management. Management of the privileges a system's access role has, enforcing security policies (password management), auditing policies and reducing the attack surface a system faces.

#### Security Models
__Bell-La Padula__ (Confidentiality) - no write down, no read up
__Biba__ (Integrity) - no write up, no read down
