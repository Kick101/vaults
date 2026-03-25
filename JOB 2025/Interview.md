### Technical Topics

- OWASP Top 10
- OWASP API Top 10
- Mobile app testing (static + dynamic analysis, reverse engineering basics).
- Phishing—what is it, prevention steps, how to respond if your data appears on Dark Web.
- CIA Triad (Confidentiality, Integrity, Availability).
- Hashing vs Encryption vs Encoding.
- Symmetric vs Asymmetric crypto.
- SSL/TLS basics.
- Malware types: Virus, Worm, Trojan, Ransomware.	
- Difference between **Deep Web vs Dark Web**.
- **Threat Intelligence**
	- How phishing kits, scams, and brand impersonations are identified.
	- Familiarity with IOCs (domains, IPs, hashes) and TTPs (MITRE ATT&CK).
	- Sources: VirusTotal, Shodan, Hybrid Analysis, Any.run, HaveIBeenPwned.
- **Security Products Knowledge**
	- SIEM (Microsoft Sentinel, Splunk basics).
	- Vulnerability scanners (Nessus, OpenVAS, Nmap).
	- Threat intel feeds.
- **Scripting/Automation**
	- Python/Bash for log parsing, small automation scripts (regex for phishing detection, API querying).
	- Familiarity with APIs (REST calls, JSON parsing).
- **Tools You Should Be Comfortable With**

	- **Recon:** Nmap, Amass, Subfinder.
	- **Web Pentest:** Burp Suite, OWASP ZAP, sqlmap.
	- **Cloud:** Basic AWS misconfig checks (S3 bucket exposure, IAM issues).
	- **Phishing Analysis:** Email headers, decoding base64 payloads, sandboxing suspicious files.
	- **Automation:** Python scripts (requests, BeautifulSoup, regex).

### Practical Scenarios

- **Incident Response Basics**
    
    - Steps: Identify → Contain → Eradicate → Recover → Lessons Learned.
    - Example: _“If company data leaks on Dark Web, what do you do?”_
	- Answer structure: Identify → Verify → Notify stakeholders → Contain → Monitor.

- **Threat Intelligence & Monitoring**
    
    - How attackers use Dark Web forums.
    - Why companies monitor Pastebin, Telegram, marketplaces.
    - Explain in simple language—don’t overcomplicate.

- **Sample Questions (Practice)**
    
    - Explain phishing & how to defend against it.
    - If you see unusual login attempts, how do you prioritize alerts?
    - How would you secure a web app against XSS?
    - “A client reports they received a phishing email — how do you validate and respond?”
	- “You detect a suspicious domain targeting a customer brand — what next?”
	- “How do you prioritize vulnerabilities when reporting to a client?”
	- Walk me through how you would test a web application for security flaws.
	- Explain 3 common API vulnerabilities and how to exploit/fix them.
	- How would you detect phishing campaigns?
	- What is threat intelligence, and how do you apply it in security monitoring?
	- How do you stay updated on the latest exploits/vulnerabilities?
	- Can you script something to automate subdomain discovery?
	- - Client asks: “Why should I care about this misconfigured S3 bucket?” → Translate risk in business terms.
	- Alert shows multiple login attempts from an unusual IP → Walk through your investigation.
	- You identify a vulnerability but the client downplays it — how do you handle?

### Action Items

✅ Watch a **1-hour OWASP Top 10 summary** (YouTube has many).  
✅ Write **flashcards** for terms (CIA, hashing vs encryption, OWASP).  
✅ End the day by explaining these concepts out loud—as if teaching.

---
## OWASP Top 10 2021

### A01: Broken Access Control

- **What:** Broken Access Control occurs when an application *fails to properly enforce restrictions* on what authenticated users are allowed to do. It allows attackers to act outside their intended permissions.
- **Example:** Normal user accessing admin page by modifying the URL or IDOR (Insecure Direct Object Reference).
- **Mitigation:** Enforce least privilege, implement server-side authorization checks, deny by default, avoid exposing IDs directly.

### A02: Cryptographic Failures (formerly Sensitive Data Exposure)

- **What:** Cryptographic Failures occurs when sensitive information is not adequately protected in transit, at rest, or during processing due to weak or missing cryptographic controls.
- **Example:** Storing passwords in plaintext, weak SSL/TLS, exposing credit card details.
- **Mitigation:** Use strong encryption (AES, RSA), TLS everywhere, avoid outdated protocols, don’t log sensitive data.


### A03: Injection

- **What:** Injection vulnerabilities occur when *untrusted data is sent to an interpreter as part of a command or query*. The attacker’s input tricks the interpreter into executing unintended commands or accessing unauthorized data.
- **Example:** SQL Injection, NoSQL Injection, OS Command Injection.
- **Mitigation:** Use parameterized queries/ORM, input validation, stored procedures, least privilege DB accounts.

### A04: Insecure Design

- **What:** Insecure Design refers to *flaws in the application’s architecture* or logic that create inherent security weaknesses, regardless of how well the code is implemented. Unlike coding bugs, these are issues with _how the system is designed_ to handle security.
- **Example:** No rate-limiting on login attempts, business logic flaws    
- **Mitigation:** Secure design principles (threat modeling, abuse-case testing, defense-in-depth).

### A05: Security Misconfiguration

- **What:** Security Misconfiguration occurs when applications or infrastructure are left with insecure default settings, unnecessary features, or inconsistent configurations. For example, leaving an S3 bucket publicly accessible and exposing detailed error messages to users.    
- **Example:** Default admin credentials, verbose error messages exposing stack traces.
- **Mitigation:** Harden configs, disable unused features, regular patching, automated config management.

### **A06: Vulnerable and Outdated Components**

- **What:** Vulnerable and Outdated Components refers to *applications using libraries, frameworks, or systems with known vulnerabilities*. A classic example is the Log4j ‘Log4Shell’ vulnerability, where outdated logging libraries exposed millions of applications to remote code execution. The impact can be severe, including complete system compromise or data breaches, often using publicly available exploits.
- **Mitigation:**  organizations should maintain an up-to-date software inventory, monitor dependencies for new CVEs, apply patches quickly, and automate dependency checks with tools like Dependabot or OWASP Dependency Check. The principle is simple: you can’t secure what you don’t know you’re running.

### **A07: Identification and Authentication Failures** (formerly Broken Authentication)

- **What:** Identification and Authentication Failures occur when an *application doesn’t properly verify user identities* or enforce authentication mechanisms.
- **Example:**  weak passwords, missing MFA, or storing credentials in plaintext.
- **Mitigation:** Use strong password policies, MFA, secure session handling, implement proper logout/invalidation.

### **A08: Software and Data Integrity Failures**

- **What:** Software and Data Integrity Failures occur when applications or pipelines don’t ensure that updates, code, or data haven’t been tampered with. 
- **Example:** SolarWinds supply chain attack, where attackers compromised the build system to push malicious updates to thousands of customers. Other cases include downloading dependencies from untrusted sources or failing to verify digital signatures. The impact is massive since it can lead to remote code execution and compromise entire organizations.
- **Mitigation:**  To prevent this, organizations should sign and verify updates, secure CI/CD pipelines with strong authentication and least privilege, use integrity checks like checksums, and continuously monitor supply chain dependencies.


### **A09: Security Logging and Monitoring Failures**

- **What:** Security Logging and Monitoring Failures occur when applications and infrastructure do not generate, store, monitor, or alert on security-relevant events. This prevents timely detection, investigation, and response to attacks.
- **Example:** Attackers brute force login without detection, no logs for suspicious API calls.
- **Mitigation:** Enable centralized logging, monitor anomalies, alert on suspicious events, retain logs securely.

### **A10: Server-Side Request Forgery (SSRF)**

- **What:** App fetches remote resources without validating user input, attacker tricks server into accessing internal services.

- **Example:** Attacker forces server to fetch AWS metadata `http://169.254.169.254`.

- **Mitigation:** Validate/whitelist URLs, block internal requests, use network segmentation, metadata service protections.

---
## OSI Model Attacks

### 🔐 Layer 5 – **Session Layer**

- Manages **sessions (connections)** between applications.
- Responsible for **establishing, maintaining, synchronizing, and terminating** communication.


**Common Protocols**

- NetBIOS (older Windows comms)
- RPC (Remote Procedure Call)
- PPTP (Point-to-Point Tunneling Protocol – legacy VPN)
- SMB (often mapped here, though overlaps with App layer)


⚠️ **Attacks**

- **Session Hijacking** → Stealing active session tokens (e.g., cookie theft).
- **Session Fixation** → Forcing a victim to use a known session ID.
- **Replay Attacks** → Re-sending intercepted data packets to impersonate.


🛡️ **Defenses**

- Strong **session management** (random session IDs, regeneration after login).
- Use **TLS** to protect tokens in transit.
- Short **session timeouts** & automatic re-authentication.
- Implement **MFA** to make stolen sessions less useful.

### 🔐 OSI Layer 6 – **Presentation Layer**

 📌 **Definition**

- Responsible for **data translation, encryption/decryption, and compression** between application and network.    
- Ensures that data sent by the app layer of one system can be read by the app layer of another.


 📡 **Common Protocols**

- SSL/TLS (encryption & security)
- JPEG, GIF, MPEG (encoding formats)
- ASCII, EBCDIC (character encoding)

 ⚠️ **Attacks**

- **SSL Stripping** → Downgrading HTTPS to HTTP.
- **Man-in-the-Middle (MITM)** → Attacker intercepts and modifies encrypted traffic.
- **Downgrade Attacks** → Forcing weaker cipher suites (e.g., TLS → SSLv3).
- **Malformed Encoding Attacks** → Exploiting poor decoding/validation.
    

 🛡️ **Defenses**

- Enforce **modern encryption (TLS 1.2/1.3)**.
- Use **HSTS (HTTP Strict Transport Security)** to block SSL stripping.
- Disable weak ciphers/protocols (e.g., SSLv2/SSLv3, RC4).
- Proper **certificate validation** and pinning.

| **Layer**          | **Examples of Attacks**                                                    | **Defenses / Mitigations**                                                                                  |
| ------------------ | -------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **7. Application** | SQL Injection, XSS, CSRF, Remote Code Execution, Phishing                  | Input validation, WAF, secure coding, authentication & authorization, patching                              |
| **4. Transport**   | TCP SYN Flood (DoS), Port scanning, Session reset attacks                  | Firewalls, IDS/IPS, rate limiting, SYN cookies, secure protocols (TLS)                                      |
| **3. Network**     | IP Spoofing, ICMP flood, Routing attacks (BGP hijack), DoS/DDoS            | Firewalls, anti-spoofing filters, VPNs, IDS/IPS, DDoS protection                                            |
| **2. Data Link**   | MAC spoofing, ARP poisoning, VLAN hopping                                  | Port security, Dynamic ARP Inspection, DHCP snooping, segmentation, 802.1X authentication                   |
| **1. Physical**    | Wiretapping, Jamming (Wi-Fi interference), Hardware theft, TEMPEST attacks | Physical security controls (locks, CCTV, guards), shielding cables, Faraday cages, redundant infrastructure |

---
## 🌐 Deep Web vs Dark Web

| Feature        | **Deep Web**                                                                                       | **Dark Web**                                                                                                    |
| -------------- | -------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Definition** | Portion of the internet not indexed by search engines (Google, Bing, etc.)                         | Encrypted part of the internet that requires special tools (like Tor, I2P) to access                            |
| **Size**       | Much larger than surface web (~90% of the internet)                                                | Small fraction of the deep web                                                                                  |
| **Access**     | Normal browsers, but requires credentials/subscription                                             | Requires special software (Tor browser, I2P, Freenet)                                                           |
| **Examples**   | Online banking portals, academic databases (JSTOR, IEEE), private corporate intranets, email inbox | Black markets (drugs, weapons, stolen data), hacker forums, whistleblowing sites, privacy-focused communication |
| **Legality**   | Legal (just private/restricted access data)                                                        | Mixed: accessing is not illegal, but most activities conducted are (illegal marketplaces, cybercrime)           |
| **Use Cases**  | Security → Protect personal data, confidential research, subscription services                     | Positive → Whistleblowing, privacy protection. Negative → Cybercrime, fraud, illegal trade                      |

---
## 🎣 Phishing

- **Definition:** A type of **social engineering attack** where attackers trick users into revealing sensitive information (passwords, banking details, personal data) *by pretending to be a trusted entity* (bank, company, colleague).
    
- **Common Methods:**
    
    - **Email phishing** (fake login links, invoices)
    - **Spear phishing** (targeted at specific individuals/orgs)
    - **Smishing** (SMS phishing)
    - **Vishing** (voice phishing)
    - **Business Email Compromise (BEC)**

### 🛡️ Prevention Steps

1. **Awareness & Training** → Educate users to spot phishing signs (suspicious links, urgency, typos).
2. **Email Security** → Enable spam filters, block suspicious domains.
3. **Multi-Factor Authentication (MFA)** → Even if credentials are stolen, MFA protects accounts.
4. **Verify URLs** → Hover before clicking, check HTTPS, avoid shortened links.
5. **Don’t share sensitive info via email/SMS**.    
6. **Keep software updated** → Browsers, email clients, security patches.
7. **Use anti-phishing tools** → Browser plugins, endpoint protection.

### 🚨 How to Respond if Data Appears on the Dark Web

1. **Confirm Exposure**
    - Use tools like **HaveIBeenPwned**, or dark web monitoring services.
    - Verify what data is leaked (emails, passwords, banking info).

2. **Contain the Damage**
    - **Change all affected passwords immediately** (and enable MFA).
    - **Revoke API keys / tokens** if corporate accounts are compromised.
    - Inform your **bank/credit card provider** if financial data is exposed.

3. **Monitor & Report**
    - Monitor accounts for unusual activity.
    - Set up alerts for identity theft.
    - Report phishing attempts to IT/security team (if corporate) or to authorities (e.g., CERT-In (Computer Emergency Response Team) in India, FTC in US).
    
4. **Long-Term Actions**
    - Use **password manager** → unique, strong passwords.
    - Regular **credit monitoring/credit freeze** if financial info leaked.
    - Consider **identity theft protection services** if exposure is severe.

---
## 🔑 OWASP API Top 10 (2023)

1. **API1 – Broken Object Level Authorization (BOLA)**
    - Attackers access/modify other users’ data by manipulating object IDs.

2. **API2 – Broken Authentication**
    - Weak/misimplemented auth → token theft, brute force, session hijack.

3. **API3 – Broken Object Property Level Authorization**    
    - Unauthorized access to sensitive fields/properties in objects.

4. **API4 – Unrestricted Resource Consumption**
    - No rate limiting → DoS, resource exhaustion.

5. **API5 – Broken Function Level Authorization**
    - Attackers call admin/debug functions without proper authorization.

6. **API6 – Unrestricted Access to Sensitive Business Flows**
    - Exploiting logic flaws → abuse business operations (e.g., coupon abuse).

7. **API7 – Server-Side Request Forgery (SSRF)**
    - API fetches attacker-controlled URLs → access internal services.

8. **API8 – Security Misconfiguration**
    - Default creds, verbose errors, improper headers, mis-set CORS.

9. **API9 – Improper Inventory Management**
    - Exposed undocumented/old API versions → shadow APIs, forgotten endpoints.

10. **API10 – Unsafe Consumption of APIs**
	- Trusting external APIs blindly → injection, malicious data propagation.

---
## Cyber kill chain

The Cyber Kill Chain, developed by Lockheed Martin, breaks an attack into seven stages. It helps security teams understand and *disrupt attacks at each phase*. Compared to MITRE ATT&CK, which provides a detailed matrix of adversary tactics and techniques, the Kill Chain is more of a high-level framework.

### **7 Phases of the Cyber Kill Chain**

1. **Reconnaissance**
    
    - Attacker gathers info about the target (e.g., OSINT, scanning).
    - Example: Identifying open ports, exposed emails.
        
2. **Weaponization**
    
    - Crafting a malicious payload.
    - Example: Creating an exploit-laden PDF.
        
3. **Delivery**
    
    - Transmitting the payload to the target.
    - Example: Phishing email, USB drop.
        
4. **Exploitation**
    
    - Payload triggers, exploiting a vulnerability.
    - Example: Remote code execution.
        
5. **Installation**
    
    - Malware / backdoor gets installed for persistence.
    - Example: Installing a RAT.
        
6. **Command & Control (C2)**
    
    - Attacker establishes remote communication.
    - Example: Beaconing to attacker-controlled server.
        
7. **Actions on Objectives**
    
    - Final stage → attacker achieves their goal.
    - Example: Data exfiltration, destruction, lateral movement.

---
## Common Qs

1. Why CloudSEK?

I want to join CloudSEK because CloudSEK’s focus on AI-driven threat detection — products like XVigil and BeVigil directly map to what excites me: threat intelligence, red teaming, and automation. I also like that it's a Indian cybersecurity organization not only providing service but building useful products. Whatever that cloudSEK is doing -- very much aligns with interests. 

**XVigil (Digital Risk Monitoring)**

> “XVigil continuously *monitors an organization’s digital footprint* — domains, apps, code leaks, credentials, even chatter on dark web — to detect *scams, phishing, brand impersonation, and data leaks*. It enriches threats with intelligence and even helps with takedowns.”

**BeVigil (Attack Surface Monitoring)** 

> “BeVigil focuses on *attack surface visibility*. It *scans* web, mobile, network, and cloud assets — even unknown shadow assets — to identify *misconfigurations*, weak encryption, and *vulnerabilities* that attackers could exploit.”

**SVigil (Software Supply Chain Risk Monitoring)**

> “SVigil looks at third-party risks. It monitors open-source components, cloud services, and dependencies for vulnerabilities or integrity issues that could affect the supply chain.”

2.  How do you manage stress in a 24/7 environment?

I understand that a 24/7 security environment can be high-pressure, especially when incidents need immediate response. I manage stress by following a structured approach — prioritizing alerts based on severity, relying on SOPs and playbooks to avoid panic, and escalating when needed. Outside of work, I keep myself balanced through exercise and short breaks to reset during shifts. I also see stress as a sign of responsibility in cybersecurity — it reminds me that the work we do has real impact, which helps me stay focused rather than overwhelmed.

3. Tell me about a project where you automated a security process.

In my SOC lab project, I built a custom Elastic Stack with Suricata as the NIDS. A key pain point was manually analyzing large Suricata logs for alerts. To automate this, I wrote Python scripts that parsed JSON logs, applied filters for critical events, and pushed them into Kibana dashboards. I also automated enrichment using threat intel feeds, so malicious IPs and domains were flagged in real-time. The result was a significant reduction in analysis time and a streamlined workflow for detecting and investigating alerts.

4. Why didn’t you take the TCS role?

I did receive an offer from TCS, but during allocation I was assigned a role that was not aligned with cybersecurity, which is where my certifications and interests lie. Rather than moving forward with a role that didn’t match my skills, I decided to focus on strengthening my cybersecurity foundation — setting up labs, working on detection projects, and participating in CTFs. This way, I stayed aligned with my long-term career goals and prepared myself for a more security-focused role like this one.

5.  How do you stay updated?

6. How would you respond to an incident?

 Use **IR lifecycle** (Detect → Contain → Eradicate → Recover → Lessons Learned).

First, I’d validate the alert to confirm it’s not a false positive. Once confirmed, I’d assess scope and severity, then contain the threat — for example, isolating the affected host from the network. Next, I’d analyze logs and forensics to identify root cause, eradicate the malicious artifacts, and patch vulnerabilities. After recovery, I’d document the incident, share lessons learned, and update playbooks or detection rules to prevent recurrence.

7. How do you prioritize alerts?

I prioritize based on *business impact* and threat severity. For example, an alert indicating privilege escalation on a production server would take priority over a malware detection on a test machine. I also look at context — is this alert associated with a known campaign or repeated attempts? Correlation across logs and threat intel helps reduce noise. Essentially, I triage alerts by criticality, scope, and exploitability, so the highest risk issues get attention first.

8. what happens when you type url in browser?

```
domain.com - browser cache - router cache - isp cache - dns server
```

9. How DNS works



### Common Terms

- **Risk** is the potential for loss, damage, or disruption when a threat exploits a vulnerability in an asset.
- **Threat** → Something that could cause harm (e.g., attacker, malware, insider).
- **Vulnerability** → Weakness that can be exploited (e.g., outdated software, weak passwords).
- **Impact** → Business consequence (data loss, downtime, financial damage, reputational loss).



