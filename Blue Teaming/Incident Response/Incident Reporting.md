Effective incident reporting should strike a balance between granularity and accessibility, making it comprehensible to both technically savvy and non-technical audiences. 

---
## Incident Identification and Categorization
#### Identifying Security Incidents

__Primary three key sources for incident identification:__

| Source                           | Description                                                                                                                                                                                                                              |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Security Systems/Tooling inPlace | There is a wide variety of security systems and tools likely in place within your organization. Some excellent sources for identification include IDS/IPS, EDR/XDR, SIEM tools, or even<br>basic anti-virus alerts and NetFlow data.<br> |
| Human Observations               | Users may notice and report suspicious activities, unusual emails, or systems behaving abnormally.                                                                                                                                       |
| Third Party Notifications        | Partners, vendors, or even customers might inform organizations about any vulnerabilities or breaches they are experiencing.                                                                                                             |

#### Categorising Security Incidents

| Incident Type       | Description                                                                                        |
| ------------------- | -------------------------------------------------------------------------------------------------- |
| Malware             | Malicious software encompassing viruses, worms, and ransomware.                                    |
| Phishing            | Fraudulent endeavors to exfiltrate sensitive information, predominantly via email                  |
| DDoS Attacks        | Deliberate attempts to inundate a system or network, thereby disrupting its regular functionality. |
| Unauthorised Access | Incidents where unauthorized entities gain access to systems or data repositories.                 |
| Data Leakage        | Inadvertent exposure of confidential data, both within and outside the organizational perimeter.   |
| Physical Breach     | Unauthorized physical access to secure locations.                                                  |

#### Incident Severity Levels
| Severity Level | Description                                                                                                             |
| -------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Critical (P1)  | Imminent threats that jeopardize core business functionalities or sensitive data, necessitating immediate intervention. |
| High (P2)      | Latent threats to business operations that, while not immediately detrimental, are of elevated priority.                |
| Medium (P3)    | Incidents that, although not posing an immediate threat to business operations, warrant timely attention.               |
| Low (P4)       | Trivial incidents or routine anomalies that can be managed within standard operational workflows.                       |
It's crucial to recognize that incidents frequently straddle multiple categories and can dynamically shift in both category and severity as additional intelligence is garnered during the analysis phase. The fluid nature of these threats mandates a flexible yet structured approach to both identification and categorization.

---
## The Incident Reporting Process

#### 1. Initial Detection & Acknowledgement
Before any incident can be formally reported, it must first be detected and acknowledged. Detection vectors can vary, ranging from human observation to automated alerts generated
by deployed security tools. In some cases, the threat actor themselves may trigger the detection, especially if you're dealing with a ransomware incident.

#### 2. Preliminary Analysis
During this phase, the scope and potential ramifications of the security incident must be ascertained. The incident should be **categorized** based on our previously established classification and severity metrics.

#### 3. Incident Logging
Every facet, action, and observation related to the security incident should be meticulously logged using an established system. Popular platforms include JIRA and TheHive Project. In the absence of such a system, alternative methods should be employed. Even rudimentary tools like pen and paper or a spreadsheet can suffice in a pinch.

#### 4. Notification of Relevant Parties
Stakeholders must be promptly identified, and notifications should be segmented into:
- **Internal Communications** - Relevant internal departments, such as IT, legal, PR, and executive teams, should be alerted. In cases where the incident has widespread and severe implications, an organization-wide notification may be warranted.
- **External Communications** - Depending on the incident's nature and impact, external communications may be necessary. This could involve notifying customers, partners, regulatory bodies, or even the general public.

#### 5. Detailed Investigation & Reporting
The duration of this phase can vary significantly, ranging from a couple of days to potentially years. What's crucial here is a comprehensive technical analysis coupled with a compilation of all findings. This in-depth investigation is vital for understanding the incident's full impact.

#### 6. Final Report Creation
The culmination of your role as an incident responder is the creation of a finalized incident report. This document will furnish regulators, insurers, and executive leadership with a detailed account of the incident, its origins, and the remedial actions taken.

#### 7. Feedback Loop!
Post-incident reflection is essential for enhancing our preparedness for future incidents. This involves revisiting and analyzing the incident to identify areas for improvement.

---
## Elements of an Incident Report

### Executive Summary
Let's consider the Executive Summary as the gateway to our report, *designed to cater to a broad audience, including non-technical stakeholders*. This section should furnish the reader with a succinct overview, key findings, immediate actions executed, and the impact on stakeholders. Since many stakeholders may only peruse the Executive Summary, it's imperative to nail this section. Here's a more granular breakdown of what should be encapsulated in the Executive Summary:

| *Section*               | *Description*                                                                                                                                                                                                                                                                                                                                                |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Incident ID             | Unique identifier for the incident.                                                                                                                                                                                                                                                                                                                          |
| Incident Overview       | Provide a concise summary of the incident's events (including initial detection) and explicitly state its type. Was it a ransomware attack, a large-scale data breach, or both? This should also encompass the estimated time and date of the incident, as well as its duration, the affected systems/data, and the status (ongoing, resolved, or escalated) |
| Key Findings            | Enumerate any salient findings that emerged from the incident. What<br>was the root cause? Was a specific CVE exploited? What data, if any,<br>was compromised, exfiltrated, or jeopardized?                                                                                                                                                                 |
| Immediate Actions Taken | Outline the immediate response measures taken. Were the affected<br>systems promptly isolated? Was the root cause identified? Did we engage any third-party services, and if so, who were they?                                                                                                                                                              |
| Stakeholder Impact      | Assess the potential impact on various stakeholders. For instance, did any customers experience downtime, and what are the financial<br>ramifications? Was employee data compromised? Was proprietary<br>information at risk, and what are the potential repercussions?                                                                                      |

### Technical Analysis

This section is where we dive deeply into the technical aspects, dissecting the events that transpired during the incident. It's likely to be the most voluminous part of the incident report. The following key points should be addressed:

##### Affected Systems & Data

Highlight all systems and data that were either potentially accessed or definitively compromised during the incident. If data was exfiltrated, specify the volume or quantity, if ascertainable.

##### Evidence Sources & Analysis

Emphasize the evidence scrutinized, the results, and the analytical methodology employed. For instance, if a compromise was confirmed through web access logs, include a screenshot for documentation. Maintaining evidence integrity is crucial, especially in criminal cases. A best practice is to hash files to ensure their integrity.

##### Indicators of Compromise (IoCs)

IoCs are instrumental for hunting potential compromises across our broader environment or even among partner organizations. It might also be feasible to attribute the attack to a specific threat group based on the IoCs identified. These can range from abnormal outbound traffic to unfamiliar processes and scheduled tasks initiated by the attacker.

##### Root Cause Analysis

Within this section, detail the root cause analysis conducted and elaborate on the underlying *cause of the security incident* (vulnerabilities exploited, failure points, etc.).

##### Technical Timeline

This is a pivotal component for comprehending the incident's sequence of events. The timeline should include:

- Reconnaissance
- Initial Compromise
- C2 Communications
- Enumeration
- Lateral Movement
- Data Access & Exfiltration
- Malware Deployment or Activity (including Process Injection and Persistence)
- Containment Times
- Eradication Times
- Recovery Times

##### Nature of the Attack

Deep-dive into the type of attack, as well as the tactics, techniques, and procedures (TTPs) employed by the attacker.

### Impact Analysis

Provide an evaluation of the *adverse effects* that the incident had on the organization's data, operations, and reputation. This analysis aims to quantify and qualify the extent of the damage caused by the incident, identifying which systems, processes, or data sets have been compromised. It also assesses the potential business implications, such as financial loss, regulatory penalties, and reputational damage.

### Response and Recovery Analysis

Outline the *specific actions taken to contain the security incident, eradicate the threat, and restore normal operations*. This section serves as a **chronological account of the measures implemented to mitigate the impact** and prevent future occurrences of similar incidents.

#### 1. Immediate Response Actions
##### Revocation of Access
- **Identification of Compromised Accounts/Systems:** A detailed account of how compromised accounts or systems were identified, including the tools and methodologies used.
- **Timeframe:** The exact time when unauthorized access was detected and subsequently revoked, down to the minute if possible.
- **Method of Revocation :** Explanation of the technical methods used to revoke access, such as disabling accounts, changing permissions, or altering firewall rules.
- **Impact :** Assessment of what revoking access immediately achieved, including the prevention of data exfiltration or further system compromise.

##### Containment Strategy
- **Short-term Containment :** Immediate actions taken to isolate affected systems from the network to prevent lateral movement of the threat actor.
- **Long-term Containment :** Strategic measures, such as network segmentation or zero-trust architecture implementation, aimed at long-term isolation of affected systems.
- **Effectiveness :** An evaluation of how effective the containment strategies were in limiting the impact of the incident.

#### 2. Eradication Measures

##### Malware Removal
- **Identification :** Detailed procedures on how malware or malicious code was identified, including the use of Endpoint Detection and Response (EDR) tools or forensic analysis.
- **Removal Techniques :** Specific tools or manual methods used to remove the malware.
- **Verification :** Steps taken to ensure that the malware was completely eradicated, such as checksum verification or heuristic analysis.

##### System Patching
- **Vulnerability Identification :** How the vulnerabilities were discovered, including any CVE identifiers if applicable.
- **Patch Management :** Detailed account of the patching process, including testing, deployment, and verification stages.
- **Fallback Procedures :** Steps to revert the patches in case they cause system instability or other issues.

#### 3. Recovery Steps

##### Data Restoration
- **Backup Validation :** Procedures to validate the integrity of backups before restoration.
- **Restoration Process :** Step-by-step account of how data was restored, including any decryption methods used if the data was encrypted.
- **Data Integrity Checks :** Methods used to verify the integrity of the restored data.

##### System Validation
- **Security Measures :** Actions taken to ensure that systems are secure before bringing them back online, such as reconfiguring firewalls or updating Intrusion Detection Systems (IDS).
- **Operational Checks :** Tests conducted to confirm that systems are fully operational and perform as expected in a production environment.

#### 4. Post-Incident Actions

##### Monitoring

- **Enhanced Monitoring Plans :** Detailed plans for ongoing monitoring to detect similar vulnerabilities or attack patterns in the future.
- **Tools and Technologies :** Specific monitoring tools that will be employed, and how they integrate with existing systems for a holistic view.

##### Lessons Learned

- **Gap Analysis :** A thorough evaluation of what security measures failed and why.
- **Recommendations for Improvement :** Concrete, actionable recommendations based on the lessons learned, categorized by priority and timeline for implementation.
- **Future Strategy :** Long-term changes in policy, architecture, or personnel training to prevent similar incidents.

---
### Diagrams
Given that the narrative can become exceedingly complex, visual aids can be invaluable for
simplifying the incident's intricacies:

- **Incident Flowchart**
	- Illustrate the attack's progression, from the initial entry point to its propagation throughout the network.
- **Affected Systems Map**
	- Depict the network topology, accentuating the compromised nodes. Use color-coding or annotations to indicate the severity of each compromise.
- **Attack Vector Diagram**
	- Utilize arrows, nodes, and annotations to trace the attacker's navigation and (post-)exploitation activities through our defenses visually.


---
### Appendices

This section serves as a repository for supplementary material that provides additional context, evidence, or technical details that are crucial for a comprehensive understanding of the incident, its impact, and the response actions taken. This section is often considered the backbone of the report, offering raw data and artifacts that can be independently verified, thus adding credibility and depth to the narrative presented in the main body of the report.

**The Appendices section may include:**
- Log Files
- Network Diagrams (pre-incident & post-incident)
- Forensic Evidence (disk images, memory dumps, etc.)
- Code snippets
- Incident Response Checklist
- Communication Records
- Legal and Regulatory Documents (compliance forms, NDAs signed by external consultants, etc.)
- Glossary and Acronyms

---
### Best Practices

- **Root Cause Analysis :** Always aim to find the root cause of the incident to prevent
- **Community Sharing :** Share non-sensitive details with a community of defenders to improve collective cybersecurity.
- **Regular Updates :** Keep all stakeholders updated regularly throughout the incident response process.
- **External Review :** Consider third-party cybersecurity specialists to validate findings.
---
## Navigating Communication Channels During Cybersecurity Incidents

When we're hit with a cybersecurity incident, the way we communicate becomes a linchpin for both our security posture and our compliance standing. Let's dissect the technical landscape of these communication channels and their intertwined implications.

### Security Dimensions of Communication Channels

- **Encryption**:  
  We must ensure that every piece of information we share is wrapped in robust end-to-end encryption. This is non-negotiable, especially when we're discussing the nitty-gritty of the incident—like which systems took a hit or which vulnerabilities got exploited.

- **Authentication and Authorization**:  
  Access to our communication channels should be as tight as Fort Knox. We can't stress enough the importance of multi-factor authentication (MFA) to double-check the identities of those trying to access the channel.

- **Data Integrity**:  
  We need to be certain that our messages remain unaltered during transit. Cryptographic hashing techniques can be our best bet to ensure the integrity of our communications.

- **Ephemeral Communications**:  
  For those top-secret discussions, we might consider using messaging platforms that auto-destruct messages post-reading. This minimizes the risk of any prying eyes accessing our sensitive data later on.

- **Air-Gapped Communications**:  
  In situations where we suspect our primary communication backbone might be under threat, we might need to resort to air-gapped systems. These systems are our last line of defense, completely isolated from other potentially compromised networks.

### Regulatory Dimensions of Communication Channels

- **Data Privacy Laws**:  
  We're operating in a world where data privacy regulations, like the EU's GDPR, hold significant sway. If we're discussing or sharing personal data, especially of EU residents, we need to toe the line with GDPR mandates.

- **Breach Notification Mandates**:  
  Certain jurisdictions have clear-cut timelines for breach notifications. We need to be aware of these when communicating about data breaches, ensuring we're not only timely but also adhering to the content guidelines set by these laws.

- **Record-Keeping**:  
  While we might lean towards ephemeral messages for security, some regulations mandate a clear record of all incident-related communications. It's a tightrope walk, but we need to find that balance.

- **Cross-Border Communications**:  
  When our incident spills over national borders, the communication game changes. Some nations have stringent data sovereignty laws, dictating the hows and wheres of data transmission and storage.

- **Chain of Custody**:  
  If there's even a hint that legal actions might follow the incident, we need to maintain an unbroken chain of custody for all communications.

---














