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
- Very near. No more than 2inches
- Payments, building access control systems

#### TCP/IP



