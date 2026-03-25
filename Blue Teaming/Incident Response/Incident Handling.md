>#### Incident handling has two main activities: investigating and recovering 

__The investigation aims to:__
- Discover the initial 'patient zero' victim and create an (ongoing if still active) incident timeline
- Determine what tools and malware the adversary used.
- Document the compromised systems and what the adversary has done
---
## Preparation Stage - Establishing incident Handling
In the preparation stage, we have two separate objectives. 
1.  ***Establishment of incident handling capability within the organization.*** 
2. The second is the ability to protect against and prevent IT security incidents by ***implementing appropriate protective measures***. 

**Such measures includes** 
Endpoint and server hardening, active directory tiering, multi-factor authentication, privileged access management. While protecting against incidents is not the responsibility of the incident handling team, this activity is fundamental to the overall success of that team.

#### Preparation Prerequisites
During the preparation, we need to ensure that we have:
- Skilled **incident handling team** members (incident handling team members can be outsourced, but a basic capability and understanding of incident handling are necessary in-house regardless)
- **Trained workforce** (as much as possible, through security awareness activities or other means of training)
- Clear **policies** and documentation
- **Tools** (software and hardware)

#### Clear Policies & Documentation

**Some of the written policies & documentation should contain an up-to-date version of the following information:**
- Contact information and roles of the incident handling team members
- Contact information for the legal and compliance department, management team, IT support, communications and media relations department, law enforcement, internet service providers, facility management, and external incident response team
- Incident response policy, plan, and procedures
- Incident information sharing policy and procedures
- Baselines of systems and networks, out of a golden image and a clean state environment
- Network diagrams
- Organization-wide asset management database
- User accounts with excessive privileges that can be used on-demand by the team when necessary (also to business-critical systems, which are handled with the skills needed to administer that specific system). These user accounts are normally enabled when an incident is confirmed during the initial investigation and then disabled once it is over. A mandatory password reset is also performed when disabling the users.
- Ability to acquire hardware, software, or an external resource without a complete procurement process (urgent purchase of up to a certain amount). The last thing you need during an incident is to wait for weeks for the approval of a $500 tool.
- Forensic/Investigative cheat sheets.

## Preparation Stage - Protective measures
Another part of the preparation stage is to **protect against incidents**. While protection is not necessarily the responsibility of an incident handling team, any protection-related activities should be known to them to better **understand the type and sophistication of an incident and know where to look for artifacts/evidence**, that could aid the investigation. 
 
> ### Some of the highly recommended protective measures, which have a high mitigation impact against the majority of threats.

#### DMARC ([[Spoofing#DMARC Record]])
>Thorough testing is mandatory; otherwise (and this is oftentimes the case), you risk blocking legitimate emails with no ability to recover them.

DMARC is an email protection against phishing built on top of the already existing SPF and DKIM.

#### Endpoint Hardening & EDR
There are a few widely recognized endpoint hardening standards by now, with **CIS and Microsoft baselines** being the most popular.
- Disable LLMNR/NetBIOS
- Implement LAPS and remove administrative privileges from regular users
- Disable or configure PowerShell in "ConstrainedLanguage" mode
- Enable Attack Surface Reduction (ASR) rules if using Microsoft Defender
- Implement whitelisting. We know this is nearly impossible to implement. Consider at least blocking execution from user-writable folders (Downloads, Desktop, AppData, etc.). These are the locations where exploits and malicious payloads will initially find themselves. Remember to also block script types such as .hta, .vbs, .cmd, .bat, .js, and similar. Please pay attention to LOLBin files while implementing whitelisting. Do not overlook them; they are really used in the wild as initial access to bypass whitelisting.
- Utilize host-based firewalls. As a bare minimum, block workstation-to-workstation communication and block outbound traffic to LOLBins
- Deploy an EDR product. At this point in time, AMSI provides great visibility into obfuscated scripts for antimalware products to inspect the content before it gets executed. It is highly recommended that you only choose products that integrate with AMSI.

#### Network Protection
- **Network segmentation** is a powerful technique to avoid having a breach spread across the entire organization. Business-critical systems must be isolated, and connections should be allowed only as the business requires. Internal resources should really not be facing the Internet directly (unless placed in a DMZ).
- Consider **IDS/IPS** systems. Their power really shines when SSL/TLS interception is performed so that they can identify malicious traffic based on content on the wire and not based on reputation of IP addresses, which is a traditional and very inefficient way of detecting malicious traffic.
- Additionally, ensure that **only organization-approved devices can get on the network**. Solutions such as **802.1x** can be utilized to reduce the risk of bring your own device (BYOD) or malicious devices connecting to the corporate network.


#### Privilege Identity Management / MFA / Passwords
- Stealing privileged user credentials is the most common escalation path in Active Directory environments
- It is recommended to teach employees to **use pass phrases** because they are harder to guess and difficult to brute force.
- **Multi-factor authentication** (MFA) is another identity-protecting solution that should be implemented at least for any type of administrative access to ALL applications and devices.

#### Vulnerability Scanning
Perform **continuous vulnerability scans of your environment and remediate** at least the "high" and "critical" vulnerabilities that are discovered. While the scanning can be automated, the fixes usually require manual involvement. If you can't apply patches for some reason, definitely segment the systems that are vulnerable!

#### User Awareness Training
Training users to recognize suspicious behavior and report it when discovered is **a big win for us**. While it is unlikely to reach 100% success on this task, these trainings are known to significantly reduce the number of successful compromises. **Periodic "surprise" testing should also be part of this training**, including, for example, monthly phishing emails, dropped USB sticks in the office building, etc.

#### Active Directory Security Assessment
The **best way to detect security misconfigurations or exposed critical vulnerabilities is by looking for them from the perspective of an attacker**. Doing your own reviews (or hiring a third party) will ensure that when an endpoint device is compromised, the attacker will not have a one-step escalation possibility to high privileges on the network.
- The **more additional tools and activity an attacker is generating, the higher the likelihood of you detecting them**, so try to eliminate easy wins and low-hanging fruits as much as possible.
- Active Directory has a **few known and unique escalation paths/bugs**. New ones are quite often discovered too. Active Directory security assessments are crucial for the security posture of the environment as a whole. *Don't assume that your system administrators are aware of all discovered or published bugs*, because in reality they probably aren't.

#### Purple Team Exercises
We need to **train incident handlers** and keep them engaged. The best place to do it is inside an organization's own environment. Purple team exercises are essentially **security assessments by a red team** that either continuously or eventually inform the blue team about their actions, findings, any visibility/security shortcomings, etc.
- Such **exercises will help in identifying vulnerabilities** in an organization while testing the blue team's defensive capability in terms of logging, monitoring, detection, and responsiveness.

---
## Detection & Analysis Stage - Detection
__levels of detection by logically categorizing our network as follows__
- Detection at the network perimeter (using firewalls, internet-facing network intrusion detection/prevention systems, demilitarized zone, etc.)
- Detection at the internal network level (using local firewalls, host intrusion detection/prevention systems, etc.)
- Detection at the endpoint level (using antivirus systems, endpoint detection & response systems, etc.)
- Detection at the application level (using application logs, service logs, etc.)

#### Initial Investigation
- *Date/Time* when the incident was reported. Additionally, *who* detected the incident and/or who reported it?
- *How* was the incident detected?
- *What* was the incident? Phishing? System unavailability? etc.
- Assemble a list of impacted systems (if relevant)
- *Document* who has accessed the impacted systems and *what actions* have been taken. Make a note of whether this is an ongoing incident or the suspicious activity has been stopped.
- *Physical location*, operating systems, IP addresses and hostnames, system owner, system's purpose, current state of the system
- (If malware is involved) List of IP addresses, time and date of detection, *type of malware*, systems impacted, *export of malicious files with forensic information* on them (such as hashes, copies of the files, etc.)
- With the initially gathered information, we can start building an incident timeline.

__Example timeline of a security event__

| Date       | Time of the event | hostname    | event description                      | data source           |
| ---------- | ----------------- | ----------- | -------------------------------------- | --------------------- |
| 09/09/2021 | 13:31 CET         | SQLServer01 | Hacker tool 'Mimikatz'<br>was detected | Antivirus<br>Software |



#### Incident Severity & Extent Questions
- What is the exploitation impact?
- What are the exploitation requirements?
- Can any business-critical systems be affected by the incident?
- Are there any suggested remediation steps?
- How many systems have been impacted?
- Is the exploit being used in the wild?
- Does the exploit have any worm-like capabilities?

#### Incident Confidentiality & Communication
Incidents are very confidential topics and as such, all of the information gathered should be kept on a need-to-know basis, unless applicable laws or a management decision instruct us otherwise.

## Detection & Analysis Stage - Analysis

#### The Investigation
The investigation starts based on the initially gathered (and limited) information that contain **what we know about the incident so far**. 

A ***3-step cyclic process*** that will iterate over and over again as the investigation evolves:
- Creation and usage of indicators of compromise (**IOC**)
- **Identification** of new leads and impacted systems
- **Data collection and analysis** from the new leads and impacted systems

#### Initial Investigation Data
In order to reach a conclusion, an investigation should be based on valid leads that have been discovered not only during this initial phase but throughout the entire investigation process. The incident handling team should bring up new leads constantly and not go solely after a specific finding, such as a known malicious tool. Narrowing an investigation down to a specific activity often results in limited findings, premature conclusions, and an incomplete understanding of the overall impact.
#### Creation & Usage Of IOCs
- *An indicator of compromise is a sign that an incident has occurred.* We may even obtain IOCs from third parties if the adversary or the attack is known. 
- To leverage IOCs, we will have to deploy an IOC-obtaining/IOC-searching tool. A common approach is to utilize WMI or PowerShell for IOC-related operations in Windows environments. 

>**A word of caution!** During an investigation, we have to be extra careful to **prevent the credentials of our highly privileged user(s) from being cached** when connecting to systems. More specifically, we need to ensure that only connection protocols and tools that don't cache credentials upon a successful login are utilized (such as WinRM). **Windows logons with logon type 3** (Network Logon) typically don't cache credentials on the remote systems. The best example of "know your tools" that comes to mind is "PsExec".  When "PsExec" is used with explicit credentials, those credentials are cached on the remote machine. When "PsExec" is used without credentials through the session of the currently logged on user, the credentials are not cached on the remote machine. 

#### Identification Of New Leads & Impacted Systems
After searching for IOCs, you expect to have some hits that reveal other systems with the same signs of compromise. These hits may not be directly associated with the incident we are investigating. Our IOC could be, for example, too generic. We need to identify and eliminate false positives. We may also end up in a position where we come across a large number of hits. In this case, we should prioritize the ones that can provide us with new leads after a potential forensic analysis.

#### Data Collection & Analysis From The New Leads & Impacted Systems
Once we have identified systems that included our IOCs, we will want to **collect and preserve the state of those systems** for further analysis. 
- Depending on the system, there are multiple approaches to how and what data to collect. Sometimes we want to perform a 'live response' on a system as it is running, or we may want to shut down a system and then perform any analysis on it.
- Malware analysis and disk forensics are the most common examination types. Any newly discovered and validated leads are added to the timeline, which is constantly updated. 

---
## Containment, Eradication, & Recovery Stage

#### Containment
We take action to prevent the spread of the incident. We divide the actions into short-term containment and long-term containment . *It is important that containment actions are coordinated and executed across all systems simultaneously*. Otherwise, we risk notifying attackers that we are after them, in which case they might change their techniques and tools in order to persist in the environment.

__Short-term containment__
The actions taken leave a minimal footprint on the systems on which they occur. Some of these actions can include:
- Placing a system in a isolated VLAN
- Pulling the network cable out of the system(s)
- Modifying the attacker's C2 DNS name to a system under our control or to a non-existing one. 

The actions here contain the damage and provide time to develop a more concrete remediation strategy. Additionally, since we keep the systems unaltered (as much as possible), we have the opportunity to take forensic images and preserve evidence if this wasn't already done during the investigation (this is also known as the backup substage of the containment stage). If a short-term containment action requires shutting down a system, we have to ensure that this is communicated to the business and appropriate permissions are granted.

__Long-term containment__
We focus on *persistent actions and changes*.
- Changing user passwords
- Applying firewall rules
- Inserting a host intrusion detection system
- Applying a system patch
- Shutting down systems. 

#### Eradication
Once the incident is contained, eradication is necessary to eliminate both the root cause of the incident and what is left of it to ensure that the adversary is out of the systems and network. 
- Removing the detected malware from systems
- Rebuilding some systems
- Restoring others from backup. 

During the eradication stage, we may extend the previously performed containment activities by applying additional patches, which were not immediately required. Additional system-hardening activities are often performed during the eradication stage (not only on the impacted system but across the network in some cases).

#### Recovery
In the recovery stage, we bring systems back to normal operation. Of course, the business needs to verify that a system is in fact working as expected and that it contains all the necessary data. When everything is verified, these systems are brought into the production environment. *All restored systems will be subject to heavy logging and monitoring after an incident*, as compromised systems tend to be targets again if the adversary regains access to the environment in a short period of time. 
Typical suspicious events to monitor for are:
- Unusual logons (e.g. user or service accounts that have never logged in there before)
- Unusual processes
- Changes to the registry in locations that are usually modified by malware

The recovery stage in some large incidents may take months, since it is often approached in phases. During the early phases, the focus is on increasing overall security to prevent future
incidents through quick wins and the elimination of low-hanging fruits. The later phases focus on permanent, long-term changes to keep the organization as secure as possible.

---
## Post-Incident Activity Stage
In this stage, our objective is to document the incident and improve our capabilities based on lessons learned from it. This stage gives us an opportunity to reflect on the threat by understanding what occurred, what we did, and how our actions and activities worked out. This information is best gathered and analyzed in a meeting with all stakeholders that were involved during the incident. It generally takes place within a few days after the incident, when the incident report has been finalized.

#### Reporting
The final report is a crucial part of the entire process. A complete report will contain answers to questions such as:
- What happened and when?
- Performance of the team dealing with the incident in regard to plans, playbooks, policies, and procedures
- Did the business provide the necessary information and respond promptly to aid in handling the incident in an efficient manner? What can be improved?
- What actions have been implemented to contain and eradicate the incident?
- What preventive measures should be put in place to prevent similar incidents in the future?
- What tools and resources are needed to detect and analyze similar incidents in the future?

***Such reports can eventually provide us with measurable results.*** 
- They can provide us with knowledge around *how many incidents have been handled, how much time the team spends per incident, and the different actions that were performed during the handling process*. 
- Additionally, incident reports also provide a reference for handling future events of similar nature. 
- In situations where legal action is to be taken, an incident report will also be used in court and as a source for identifying the costs and impact of incidents. 
- This stage is also a great place to train new team members by showing them how the incident was handled by more experienced colleagues. 
- The team should also evaluate whether updating plans, playbooks, policies, and procedures is necessary. 
- During the post-incident activity state, it is important that we reevaluate the tools, training, and readiness of the team, as well as the overall team structure, and not focus only on the documentation and process front.