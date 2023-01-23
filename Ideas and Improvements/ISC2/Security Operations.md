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
- symmetric
- asymmetric
- hashing

#### Logging and monitoring security events


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