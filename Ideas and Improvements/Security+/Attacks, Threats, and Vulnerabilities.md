### Compare and contrast different types of social engineering techniques.
__Social Engineering:__
An attempt by an attacker to convince someone to provide info (like a password) or perform an action they wouldn’t normally perform (such as clicking on a malicious link)

#### Social Engineering Attacks
##### Virtual Attacks
- __Phishing:__ Attempt to acquire sensitive information by disguising oneself as a trustworthy entity in an electronic communication.
- __Smishing:__ Attempt to acquire sensitive information by disguising oneself as a trustworthy entity in a _text message_.
- __Vishing:__ Attempt to acquire sensitive information over the _phone_ by disguising oneself as a trustworthy entity. 
- __Spam:__ Unsolicited messages sent in bulk, typically through email or messaging platforms. 
- __Spam over instant messaging (SPIM):__ Unsolicited messages sent in bulk _through instant messaging platforms_. 
- __Spear phishing:__ A targeted phishing attack directed towards a _specific individual or organization_. 
- __Whaling:__ A _targeted phishing attack_ directed towards _high-level executives_ or other high-profile individuals. 
- __Pharming:__ Redirecting a website's traffic to a fake website in order to steal sensitive information. 
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