### Compare and contrast different types of social engineering techniques.
__Social Engineering:__
An attempt by an attacker to convince someone to provide info (like a password) or perform an action they wouldn’t normally perform (such as clicking on a malicious link)

#### Social Engineering Attacks
##### Virtual Attacks
- __Phishing:__ Attempt to acquire sensitive information by _disguising oneself as a trustworthy entity_ in an electronic communication.
- __Smishing:__ Attempt to acquire sensitive information by disguising oneself as a trustworthy entity in a _text message_.
- __Vishing:__ Attempt to acquire sensitive information over the _phone_ by disguising oneself as a trustworthy entity. 
- __Spam:__ Unsolicited messages sent in bulk, typically through email or messaging platforms. 
- __Spam over instant messaging (SPIM):__ Unsolicited messages sent in bulk _through instant messaging platforms_. 
- __Spear phishing:__ A targeted phishing attack directed towards a _specific individual or organization_. 
- __Whaling:__ A _targeted phishing attack_ directed towards _high-level executives_ or other high-profile individuals. 
- __Pharming:__ _Redirecting a website's traffic to a fake website_ in order to steal sensitive information. Via DNS poision or DNS hijack or DNS spoofing.
- __Typosquatting:__ Registering a _domain name that is similar to a legitimate website_ in order to trick users into visiting the fake site. 
- __Watering hole attack:__ A targeted cyber attack that focuses on a specific group or organization by _compromising a website or service that is known to be frequently accessed by members of that group_ or organization. 

##### Physical Attacks
- __Dumpster diving:__ Physical retrieval of discarded materials in order to gain sensitive information. 
- __Shoulder surfing:__ Observing someone's actions, typically to gain sensitive information such as passwords. 
- __Tailgating:__ Unauthorized access to a restricted area by _following someone_ with authorized access. 

##### Both
- __Eliciting information:__ Attempting to _obtain sensitive information through conversation or other means of communication_. 
- __Prepending:__ The practice of _adding false information to the beginning_ of a message or file.
- __Identity fraud:__ The _unauthorized use of someone else's personal information_ to commit fraud or other crimes. 
- __Invoice scams:__ Attempts to trick individuals or organizations into paying for goods or services that were not ordered or received. 
- __Credential harvesting:__ Attempts to obtain login credentials for a website or service.
- __Reconnaissance:__ The process of gathering information about a target in preparation for an attack. 
- __Hoax:__ A _false message or story_, typically spread through electronic means, intended to trick or deceive people. 
- __Impersonation:__ Assuming someone else's identity in order to gain access to sensitive information or resources. 
- __Pretexting:__ Creating a _false scenario or identity_ in order to obtain sensitive information.

#### Influence Campaign
A method of manipulating public opinion through targeted messaging and disinformation.
- __Hybrid warfare:__ The use of a combination of conventional, unconventional like using _paid advertisement_, and cyber warfare tactics to achieve military objectives.
- __Social media:__ Platforms and websites that allow users to create and share content, connect with others, and participate in online communities. May involve creating multiple fake accounts to post content and seed the spread.

#### Principles of SE: reasons for effectiveness
- __Authority:__ Citing position, responsibility, or affiliation that grants the attacker the authority to make the request.
- __Intimidation:__ Suggesting you may face negative outcomes if you do not facilitate access or initiate a process.
- __Consensus:__ Claiming that someone in a similar position or peer has carried out the same task in the past.
- __Scarcity:__ Limited opportunity, diminishing availability that requires we get this done in a certain amount of time, similar to urgency. (Quantity)
- __Familiarity:__ Attempting to establish a personal connection, often citing mutual acquaintances, social proof.
- __Trust:__ Citing knowledge and experience, assisting the target with an issue, to establish a relationship
- __Urgency:__ Time sensitivity that demands immediate action, similar to scarcity

---
### Given a scenario, analyze potential indicators to determine the type of attack
#### Malware
Software designed to harm or exploit computer systems.
-   **Ransomware**: malware that _encrypts files and demands payment_ to restore access.
-   **Trojans**: malware that _disguises itself as legitimate software_ to gain access to a system.
-   **Worms**: _self-replicating_ malware that spreads through networks and can cause damage.
-   **Potentially unwanted programs (PUPs)**: software that may not be malicious, but is often _unwanted or unnecessary_.
-   **Fileless virus**: malware that _resides in the computer's memory_ and does not leave a file on the hard drive.
-   **Command and control**: a _method of remotely controlling malware_ on infected systems.
-   **Bots**: software that _automates tasks_ and can be used for malicious purposes, such as creating a botnet.
- __Bot Herder:__ criminal who uses a command-and-control server to remotely control the bots
- __Botnet:__ A collection of compromised computing devices
-   **Cryptomalware**: malware that encrypts files and demands payment in cryptocurrency.
-   **Logic bombs**: malware that is _programmed to activate at a specific time or under certain conditions_.
-   **Spyware**: software that _monitors and collects information_ about a user's computer activity without their knowledge.
-   **Keyloggers**: software that _records every keystroke_ made on a computer.
-   **Remote access Trojan (RAT)**: malware that _allows an attacker to gain remote control_ of a computer.
-   **Rootkit**: malware that _hides its presence_ on a computer and can evade detection. _Exploits known vulnerabilities in various operating systems_.
-   **Backdoor**: a method of _bypassing normal authentication or security measures_ to gain unauthorized access to a system.

#### Password Attacks
Refer to any method used to gain unauthorized access to a system or network by guessing or cracking passwords.

- **Spraying** refers to a technique used in password attacks where a small list of common passwords is used to try and gain access to multiple accounts.
- **Dictionary** attacks use a predefined list of words to try as potential passwords in a password guessing attack.
- **Brute force** attacks involve trying every possible combination of characters in order to guess a password.
	- **Offline** attacks are when an attacker gains access to hashed passwords and attempts to crack them locally, without the need to interact with the system they are trying to access.
	- **Online** attacks involve guessing or cracking passwords on the actual system or network being targeted.
- **Rainbow table** is a precomputed table for reversing cryptographic hash functions, usually for cracking password hashes.
- **Plaintext** or **unencrypted** refers to data that is not encoded or protected in any way, making it easily readable by anyone who accesses it.

#### Physical attacks
Physical attacks are any type of security breach that involves the _use of physical force or manipulation of physical devices_ to gain unauthorized access to sensitive information.
- **Malicious Universal Serial Bus (USB) cable**: A malicious USB cable is a type of hacking tool that allows an attacker to gain unauthorized access to a computer or device when the cable is plugged into it.
- **Malicious flash drive**: A malicious flash drive is a type of hacking tool that allows an attacker to gain unauthorized access to a computer or device when the flash drive is plugged into it.
- **Card cloning**: Making a clone of legitimate cards using skimmers.
- **Skimming**: Focuses on capturing/reading data from RFID and magnetic stripe cards.

#### Adversarial artificial intelligence (AI)
Refers to the use of AI techniques to create or manipulate data to deceive or disrupt other AI systems.

- __Tainted training data for machine learning (ML):__ Data poisoning that supplies AI and ML algorithms with adversarial data that serves the attackers purposes, or attacks against privacy.
- __Security of machine learning algorithms:__ Validate quality and security of the data sources. Secure infrastructure and environment where AI and ML is hosted. Review, test, and document changes to AI and ML algorithms.

#### Supply-chain attacks
- a cyber-attack that seeks to damage an organization by targeting less-secure elements in the supply chain.
- Often attempt to compromise devices, systems, or software before it reaches an organization.
- Sometimes focus on compromising a vulnerable vendors in the organization’s supply chain, and then attempting to breach the target organization.
- Also known as _Island Hoping Attack_.

#### Cloud-based Attacks
- Data centre is often more secure and less vulnerable to disruptive attacks (like DDoS) On the downside, you will not have facility level or physical system-level audit access.
####  On-premises Attacks
- You do not benefit from the cloud’s shared responsibility model. You have more control but are responsible for security of the full stack.

#### Cryptographic attacks
- __Birthday:__ An attempt to find collisions in hash functions. Commonly targets digital signatures.
- __Collision:__ Attack on a cryptographic hash to find two inputs that produce the same hash value. Beat with collision-resistant hashes
- __Downgrade:__ when a protocol is downgraded from a higher mode or version to a low-quality mode or lower version. Commonly targets TLS.
- __Replay:__ an attempt to reuse authentication requests. targets authentication (often Kerberos)

---
### Given a scenario, analyze potential indicators associated with application attacks
#### Privilege escalation
Privilege escalation is the process of a user or program _gaining access to higher level privileges_ than they are normally authorized to have.

#### Cross-site scripting (XSS)
Web vulnerability that allows an attacker to _inject malicious code_ (such as JavaScript) into a website, potentially stealing sensitive information from users visiting the affected site.

#### Injections
A method of inserting malicious code into a website or application by exploiting vulnerabilities in its input validation.
- **Structured Query Language Injection (SQL):** Attack that targets the database of a system or application. It is done by injecting malicious SQL commands into a form input, URL, or other type of user input
- **Dynamic-Link Library Injection (DLL):** Attack that targets dynamic-link libraries (DLLs) used by a system or application. It is done by injecting a malicious DLL into the system or application
- **Lightweight Directory Access Protocol Injection (LDAP):** Attack that targets Lightweight Directory Access Protocol (LDAP) servers. It is done by injecting malicious LDAP commands into a form input, URL, or other type of user input.
- **XML External Entity (XXE):** Injection of external entities into an XML document, potentially allowing them to access sensitive information or execute malicious code on the affected system.

#### Request forgeries 
- __Server-side Request Forgery (SSRF):__ Web application vulnerability that allows an attacker to _manipulate the server into making requests on their behalf_. This is achieved by injecting malicious URLs into the application, which the server then uses to make requests to other systems or internal resources.
- __Cross-site Request Forgery (CSRF):__ Web application vulnerability that allows an attacker to _manipulate a user's browser into making requests to a web application on their behalf_. This is achieved by injecting malicious code into a web page, which the user's browser then sends to the web application without the user's knowledge or consent.

#### Pointer/object dereference
- Dereferencing means accessing the data stored at a specific memory address or location in a computer program
- This attack consists of _finding null references in a program and dereferencing them, causing an exception_ to be generated and application crashes.
- The vulnerability in memory that usually causes the applications to crash or a denial of service is a _NULL Pointer dereference_.

#### Directory traversal 
Security vulnerability that _allows an attacker to access files and directories outside of the intended directory_ structure by manipulating file paths.

#### Buffer overflows
Security weakness that occurs when _more data is written to a memory buffer than it can hold, causing it to overflow_ and potentially allowing an attacker to execute malicious code.

#### Race conditions 
A condition where the system's behaviour is dependent on the sequence or timing of other uncontrollable events.
- __Time of check/time of use:__ When a program checks the state of an object (e.g. a file, a network connection) and then uses it later on, without ensuring that the state of the object has not changed in the meantime. (Ex: DNS Rebinding)

#### Error handling 
- The process of detecting and responding to errors that occur during the execution of a program.
- Properly done, the user will simply see an error message box. If a program crashes, it is a sign of poor error handling

#### Improper input handling
The failure to properly validate or sanitise user input, which can lead to security vulnerabilities such as SQL injection or cross-site scripting (XSS) attacks.

#### Replay attack
An attempt to reuse authentication requests.
- __Session replays:__ An attacker steals a valid session ID of a user and reuses it to impersonate an authorized user and perform fraudulent transactions or activities.

#### Integer overflow 
A type of arithmetic overflow error when the result of an integer operation _does not fit within the allocated memory space_, causes the result to be unexpected.

#### Application programming interface (API) attacks
- API attacks refer to methods used by attackers to _exploit vulnerabilities in application programming interfaces_ (APIs) to gain unauthorized access to systems or data, disrupt or degrade service, or steal sensitive information. 
- These attacks can include techniques such as injection attacks, authentication bypass, and denial of service (DoS) attacks.

#### Resource exhaustion
- Resource exhaustion refers to a situation where an attacker _floods an application or system with a large amount of requests_, causing it to use up all of its resources, such as memory, CPU, or network bandwidth. 
- This leads to a state where the application or system is unable to respond to legitimate requests, causing a Denial of Service. (Ex: DoS, DDoS)

#### Memory leak 
- Software bug where an _application continuously allocates memory but fails to release it back to the system_, resulting in an increasing amount of used memory. As the application continues to run, it will consume more and more memory until it eventually causes the system to slow down or crash. 
- Memory leaks are common in programs written in C and C++

#### Secure Sockets Layer (SSL) stripping
- A technique by which a website is _downgraded from https to http_ and exposes you to eavesdropping and data manipulation.
- In order to “strip” the TLS/SSL, an attacker intervenes in the redirection from HTTP to HTTPS and intercepts a request from the user to the server.


#### Driver manipulation
- Drivers are _software programs that allow the operating system to communicate with hardware devices_, such as printers, keyboards, and network adapters.
- Attacker alters or replaces the legitimate drivers installed on a system with malicious versions to gain access to sensitive information, execute arbitrary code, or cause the system to crash.
- __Shimming:__ Technique of _intercepting an application's function calls and redirecting them to a malicious library or code_. This is often achieved by creating a shim, a small library that sits between the application and the operating system, and intercepts certain function calls. Often used to bypass security controls, such as antivirus software, or to perform malicious actions, such as stealing sensitive information.
- __Refactoring:__ Software attack in which an attacker _modifies the code of an application or system in a way that changes its behavior, but does not change its functionality_. For example, a piece of malicious code inserted into a program that is designed to steal sensitive information, but the program will still perform its intended function, such as opening a document or playing a video. The malicious code will run in the background, performing its malicious function without being noticed by the user or security controls, as the functionality of the program has not been changed.

#### Pass the hash
A technique whereby an _attacker captures a password hash and then passes it through for authentication_ and lateral access.
- One primary difference between pass-the-hash and __pass-the-ticket__, is ticket expiration.
- Kerberos TGT tickets expire (10 hours by default) whereas NTLM hashes only change when the user changes their password.
> “Credential Guard” in Windows 10 encrypts hash in memory, stopping this attack

---
### Given a scenario, analyze potential indicators associated with network attacks.
#### Wireless 
- **Evil twin**: An evil twin is a type of wireless access point (AP) that masquerades as a legitimate one, usually with the intention of stealing sensitive information from unsuspecting victims.
- **Rogue access point**: A rogue access point (AP) is a wireless access point that has been installed on a network without the authorization of the network administrator.
- **Bluesnarfing**: Attacker gains _unauthorized access to data_ on a Bluetooth-enabled device.
- **Bluejacking**: Attacker _sends unsolicited messages_ to Bluetooth-enabled devices.
- __Bluebugging:__ creates a backdoor attack before returning control of the phone to its owner.
- **Disassociation**: Disassociation is the process of _disconnecting a device_ from a wireless network.
- **Jamming**: Jamming is the intentional _broadcasting of radio signals on the same frequency/channel as an authorized transmitter_, with the intent to interfere with or disrupt the legitimate wireless communications.
- **Radio frequency identification (RFID)**: RFID is a technology that _uses radio waves to identify and track objects wirelessly_. Vulnerable to sniffing, spoofing, cloning, replay, relay, and DoS attacks.
- **Near-field communication (NFC)**: NFC is a technology that enables _short-range wireless communication between devices_, typically used for mobile payments and data transfer. Subject to many of the same vulnerabilities as RFID.
- **Initialization vector (IV)**: _Modifies the initialization vector of an encrypted wireless packet during transmission_. Enables attacker to compute the RC4 key stream generated by IV used and decrypt all other packets.

#### On-path attack ( man-in-the-middle attack/ man-in-the-browser attack)
- _Attacker sits in the middle between two endpoints_ and is able to intercept traffic, capturing (and potentially changing) information.
- Examples: session hijacking, and denial-of-service (DoS) attacks.

#### Layer 2 attacks 
- __Address Resolution Protocol (ARP) poisoning:__ Sending _unsoliciated ARP replies between two endpoints that contain the attacker’s MAC address_ with endpoints IP addresses.
- __Media access control (MAC) flooding:__ Attacker _floods a network switch with a large number of fake MAC addresses_, causing the switch to overload and potentially disrupt network communication, causes a denial of service (DoS).
- __MAC cloning:__ _Duplicates the MAC address of a device_, allowing attacker to appear as a trusted device.

#### Domain name system (DNS) 
- **Domain hijacking** is the _unauthorized transfer of control over a domain name_ from its rightful owner to another party.
- **DNS poisoning:** Attacker _alters the domain-name-to-IPaddress mappings in a DNS_ system may redirect traffic to a rogue system OR perform denial-of-service against system.
- __DNS Spoofing:__ Attacker _sends false replies to a requesting system_, beating the real reply from the valid DNS server.
- **URL redirection** is the technique of _redirecting a web page request to a different URL_, which can be used for phishing or other malicious purposes.
- __URL Hijacking:__ Attacker _takes control of a website's URL and redirects visitors to  malicious website_. This can be done by typosquatting or by using different TLD.
- **Domain reputation** is a _measure of the trustworthiness of a domain_ based on various _factors such as its history of hosting malicious content, its owner, and the nature of its content_.

#### Distributed denial-of-service (DDoS)
A type of cyber attack that aims to make a website or network resource unavailable by overwhelming it with traffic from multiple sources.

- **Network**: Volume-based attacks _targeting flaws in network protocols_, often using botnets, using techniques such as UDP, ICMP flooding, or SYN flooding (TCP-based).
- **Application**: Exploit weaknesses in the application layer by opening connections and _initiating process and transaction requests that consume finite resources like disk space and available memory_.
- **Operational technology (OT)**: Targets the weaknesses of software and hardware devices that _control systems in factories, power plants, and other industries, such as IoT devices_.

#### Malicious code or script execution 
_Malicious code or scripts_ that can created in many programming languages:
- PowerShell (windows shell script)
- Python (multi-purpose programming language)
- Bash (Linux shell script)
- Macros (code that automates a repetitive tasks, used in micorsoft products mainly)
- Visual Basic for Applications (VBA) (windows shell script)

Ex: Logic bombs, macro virus, python keylogger etc.

---
### Explain different threat actors, vectors, and intelligence sources.
#### Actors and threats
- **Advanced persistent threat (APT)**: A _group of attackers conduct sophisticated series of attacks_ taking place over an _extended period of time_. Typically wellorganized, well-funded and highly skilled.
-   **Insider threats**: A security threat to an organization that comes from within, either from an _employee, a former employee, or a business partner_.
-   **State actors**: State-sponsored hackers or organizations who are _backed by a country's government_ to perform cyber attacks on other countries.
-   **Hacktivists**: Hackers who use their skills to _achieve a political or social agenda_.
-   **Script kiddies**: Individuals _who use pre-written scripts_ or programs, often _without understanding how they work_, to launch cyber attacks.
-   **Criminal syndicates**: Organized groups that engage in _cybercrime for financial gain_.
- __Hacker:__ A hacker is a person who uses computer skills to gain access to systems, networks or data.
	- __Unauthorized:__ Black Hat
	- __Authorized:__ White Hat
	- __Semiauthorized:__ Grey Hat
- __Shadow IT:__ The use of information technology systems, devices, software, applications, and services _without explicit IT department approval_, often done with no bad intentions.
- __Competitors:__ _Competitive organization attack_ to steal or destory information or intellectual property.

#### Attributes of actors
-   **Internal/External**: Whether the actor is within an organization or outside of it.
-   **Level of Sophistication/Capability**: The level of expertise and capability the actor possesses.
-   **Resources/Funding**: The resources and funding available to the actor.
-   **Intent/Motivation**: The reasons or goals driving the actor's action

#### Vectors
The _means by which a threat or vulnerability can propagate_ or spread.
-   **Direct access:** Physical access to facilities, hardware and infrastructure. Keylogger, flash drive common here.
-   **Wireless:** Wireless communication networks and devices.
-   **Email:** Electronic mail and messaging systems.
-   **Supply chain:** The flow of goods, services, and information from suppliers to manufacturers to distributors to customers.
-   **Social media:** Online platforms for social interaction and communication.
-   **Removable media:** Portable storage devices such as USB drives and CDs.
-   **Cloud:** Remote servers and storage accessed through the internet.


---
### Explain the security concerns associated with various types of vulnerabilities

---
### Summarize the techniques used in security assessments.

---
### Explain the techniques used in penetration testing.