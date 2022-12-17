### Offensive Security
-   __Penetration Tester__ - Responsible for _testing technology_ products for finding exploitable security vulnerabilities.
- __Red Teamer__ - Plays the _role of an adversary_, attacking an organization and providing feedback from an enemy's perspective.
- __Security Engineer__ - _Design, monitor, and maintain_ security controls, networks, and systems to help prevent cyber-attacks.

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

The four major phases of the incident response process are:
1.  Preparation: This requires a team trained and ready to handle incidents. Ideally, various measures are put in place to prevent incidents from happening in the first place.
2.  Detection and Analysis: The team has the necessary resources to detect any incident; moreover, it is essential to further analyze any detected incident to learn about its severity.
3.  Containment, Eradication, and Recovery: Once an incident is detected, it is crucial to stop it from affecting other systems, eliminate it, and recover the affected systems. For instance, when we notice that a system is infected with a computer virus, we would like to stop (contain) the virus from spreading to other systems, clean (eradicate) the virus, and ensure proper system recovery.
4.  Post-Incident Activity: After successful recovery, a report is produced, and the learned lesson is shared to prevent similar future incidents.