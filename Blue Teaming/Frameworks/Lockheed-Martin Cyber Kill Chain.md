
| *Technique*               | *Purpose*                                                                                                                | *Examples*                                                |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------- |
| **Reconnaissance**        | Obtain information about the victim and the tactics used for the attack.                                                 | Harvesting emails, OSINT, and social media, network scans |
| **Weaponisation**         | Malware is engineered based on the needs and intentions of the attack.                                                   | Exploit with a backdoor, malicious office document        |
| **Delivery**              | Covers how the malware would be delivered to the victim's system.                                                        | Email, weblinks, USB                                      |
| **Exploitation**          | Breach the victim's system vulnerabilities to execute code and create scheduled jobs to establish persistence.           | EternalBlue, Zero-Logon, etc.                             |
| **Installation**          | Install malware and other tools to gain access to the victim's system.                                                   | Password dumping, backdoors, remote access trojans        |
| **Command & Control**     | Remotely control the compromised system, deliver additional malware, move across valuable assets and elevate privileges. | Empire, Cobalt Strike, etc.                               |
| **Actions on Objectives** | Fulfil the intended goals for the attack: financial gain, corporate espionage, and data exfiltration.                    | Data encryption, ransomware, public defacement            |

## 🧷 Attack Chain – CVE-2017-1000353 (Monero Mining Campaign)

| **Phase**                 | **Indicators / Actions**                                                   |
| ------------------------- | -------------------------------------------------------------------------- |
| **Reconnaissance**        | - Scanning for public-facing Jenkins servers (e.g., via Shodan or masscan) |
| **Weaponization**         | - PowerShell script crafted to download and execute the Monero miner       |
|                           | - Malicious payload hosted and ready for retrieval via Jenkins CLI exploit |
| **Delivery**              | - HTTP POST to Jenkins CLI endpoint with a crafted serialized Java object  |
|                           | exploiting CVE-2017-1000353                                                |
| **Exploitation**          | - Deserialization vulnerability triggers remote code execution             |
|                           | - PowerShell invoked through Jenkins CLI to run payload                    |
| **Installation**          | - Drops `minerxmr.exe` to `C:\Windows\minerxmr.exe`                        |
| **Command & Control**     | - Connects to external C2: `222.184.79.11:5329`                            |
| **Actions on Objectives** | - Starts mining Monero cryptocurrency using victim system resources        |

## Petya

