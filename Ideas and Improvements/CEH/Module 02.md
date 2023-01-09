## Foot Printing and Reconnaissance
At the end of this module, you will be able to:
- Describe footprinting concepts
- Perform footprinting through search engines and using advanced Google hacking techniques
- Perform footprinting through web services and social networking sites
- Perform website footprinting and email footprinting
- Perform Whois, DNS, and network footprinting
- Perform footprinting through social engineering
- Use different footprinting tools
- Apply footprinting best practices
---
### Footprinting Concepts
>Footprinting is the process of gathering information about a target system or organization in order to identify potential vulnerabilities that can be exploited during a cyber attack. It is an important part of the ethical hacking process because it helps hackers to understand the target environment and plan their attacks accordingly. Footprinting can involve a variety of activities, such as gathering information about the organization's structure and business operations, identifying key personnel and their roles, and researching the technology and software that the organization uses. By understanding these details, hackers can identify potential weak points in the organization's defenses and tailor their attacks to exploit them.

__Footprinting Basic Terminology__
- Why do we do footprinting?
	- To gather information about a target system or an organization.
- What is the need of this information?
	- We can analyze this information to find any vulnerabilities in the system
- What are these types of information?
	- Organisation structure
	- Business operations
	- Identify key personnel and their roles in the org.
	- System technology and software
	- Network information
- What are the types of footprinting?
	- Active footprinting
	- Passive footprinting
- What is passive footprinting?
	- Gathering information _without interaction_ with target.
- What is the need for passive footprinting?
	- It is hard to be detected by the target
- How to perform passive footprinting?
	- It is performed by collecting archived and stored information using search engines, social engines(Google, Shodan), social networking sites, so on.
- What is active footprinting?
	- Gathering information _with direct interaction_ with target.
- What is the need for active footprinting?
	- It gathers information that is not available using passive footprinting technique
	- Information like: open ports, service enumeration, directories
- How to perform active footprinting?
	- Using tools that directly interact with the target system
	- Such as: nmap, nc, nessus, metasploit, and other enumeration tools
- What is difference b/w active and passive footprinting?
	- Active is more prone to be detected by the target unlike passive
	- Passive doesn't interact with system directly to gather info unlike active

__Information Obtained in footprinting__

![[Pasted image 20230109203144.png]]

- How can we obtain this information?
	- Organisation Information
		- Analyse org website and query Whois database
	- Network Information
		- Whois DB, trace routing, DNS etc
	- System Information
		- DNS, web, email footprinting
- What are the _footprinting threats_?
	- Using footprinting we can do all the below:
		- Social Engineering
		- System and Network attacks
		- Information leakage
		- Privacy loss
		- Corporate Espionage
		- Business loss
---
### Footprinting Methodology

![[Pasted image 20230109205016.png]]

#### Footprinting through Search Engines
- How does search engines work?
	- Search engines use software called crawlers to continuously scan active websites and add the  retrieved results in the search engine index that is further stored in a massive DB.
	- When user queries the search engine index, it returns a list of SERPs (Search Engine Results Pages)
- What information can we gather using this?
	- 
- How can we perform this?
	- We can perform this using search engine operators to query results 

__Google Hacking__
- 