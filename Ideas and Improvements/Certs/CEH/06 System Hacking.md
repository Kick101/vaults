- Techniques of gaining access
- Privilege Escalation
- Gaining remote access
- Rootkits
- Stenography and Steganalysis
- Covering tracks
- Countermeasures
---
# Gaining Access
### Cracking Passwords
#### Microsoft Authentication
When user logs into a windows system. Windows OS authenticates it's users with three mechanisms.

##### Security Accounts Manager (SAM) Database
- Passwords are stored in hash format
- SAM database is a _registry file_ and windows kernel keeps a filesystem lock on SAM file.
- SAM file cannot be moved/copied while the OS is running
	- Lock doesn't release until system throws a blue screen exception, or the OS has shutdown
- SAM file uses an _SYSKEY_ function to partially encrypt the password hashes.
- _Attackers can dump the SAM file._
- File location: `%SystemRoot%/system32/config/SAM`
- Registry Location: `HKLM/SAM` - LM or NTLM hashes passwords.

##### [[Red Teaming/Active Directory/Basics#NTLM Authentication]]

##### [[Red Teaming/Active Directory/Basics#Kerberos Authentication]]
- Authentication is done by using _secret-key_ cryptography.
- It's a _mutual authentication_: each other identity is verified
- This protects against _replay attacks_ and _evaesdropping_
- User identity is verified by _tickets_
- _Key Distribution Center (KDC):_ trusted third party. It has:
	- Authentication Server (AS)
	- Ticket-Granting Server (TGS)

#### Classification of Password Attacks
- Non-Electronic Attacks
- Active Online Attacks
- Passive Online Attacks: sniffing, MiTM, replay
- Offline Attacks: Rainbow tables, brute-force on hashes

##### Non-Electronic Attacks
- Social Engineering
- Dumpster diving
- Shoulder surfing

##### Active Online Attacks
__Dictionary Attack__
- Used in two situations:
	- Discover decryption key
	- Cracking password
- Methods to improve the success
	- Use different types of dictionaries
	- Use string manipulation. Ex: system -> metsys

__Brute-Force Attack__
- Every combination of characters until password is broken.
- It is a time-consuming process
- All passwords will eventually be found

__Rule-base Attack__
- Used when some information of password is known or hunch
- _Hybrid Attack_
	- Additional characters are added to the words in the dictionary
	- Ex: john@2021 -> john@2022 or john@2023
- _Syllable Attack_
	- Dictionary of syllables are used and combinations of them
	- Ex: a, ap, app, appl, b, ba, ban, c, ca, car, d, do, dog

__Password Spraying Attack__
- Targets multiple user accounts simultaneously using one or a small set of commonly used passwords. 
- Tools:
	- crackmapexec, kerbrute, invoke-DomainPasswordSpray, Omnispray, Spray

```bash
crackmapexec smb $IP -u users.txt -p passwords.txt
```

__Mask Attack__
- Pattern of the password is used

```bash
hashcat -a 3 -m 0 md5_hashes.txt -l ?l?l?l?d?d?d 
```

__Password Guessing__
- Wordlist is created manually after gaining some information
- Process:
	- Find valid user
	- Create list of possible passwords
	- Rank passwords from high to low probability
	- Crack
- Failure rate is high

__Default Passwords__
- Passwords that come with equipment
- Ex: routers, switches
- Online Tools:
	- https://open-sez.me 
	- https://www.fortypoundhead.com 
	- https://cirt.net 
	- http://www.defaultpassword.us 
	- https://www.routerpasswords.com 
	- https://default-password.info 
	- https://192-168-1-1ip.mobi

__Trojan/Spyware/Keyloggers__
- Malware runs in the background and send back all user credentials to the attacker

__Hash Injection/Pass-the-Hash (PtH) Attack__

__LLMNR/NBT-NS Poisoning__

__Internal Monologue Attack__

__Cracking Kerberos Password__

__Pass-the-Ticket Attack__

##### Passive Online Attacks

##### Offline Attacks
Time-consuming but have a high success rate

__Rainbow Table Attack__
- Pre-computed hash tables are used to compare the hash
- Tools:  http://project-rainbowcrack.com

```bash
rtgen hash_algorithm charset plaintext_len_min plaintext_len_max table_index chain_len chain_num part_index
```

__Distributed Network Attack__
- DNA manager installed in a central location where machines running DNA clients can access it over a network. 
- A DNA manager coordinates the attack and assigns small portions of the key search to machines distributed throughout the network. 
- The DNA client runs in the background, only taking the processor time that was unused. 


##### Password Recovery Tools
- https://www.elcomsoft.com
- Password Recovery Toolkit (https://accessdata.com)
- Passware Kit Forensic (https://www.passware.com)
- hashcat (https://hashcat.net)
- Windows Password Recovery Tool (https://www.windowspasswordsrecovery.com)
- PCUnlocker (https://www.top-password.com)

##### Password Extracting / Hash Dumping
- pwdump7, mimikatz, powershell empire, DSInternals PowerShell, Ntdsxtract

__Local Security Authority Subsystem Service (LSASS)__
Responsible for _authentication, password management, active directory, security policy enforcement_.
- _Loaded into memory_ when a user logs in to the system
- "Pass-the-Hash" attack
- Dumping password hashes

__Dumping using registry__
_Victim_
```powershell
reg save hklm\system system
reg save hklm\sam sam
```
_Attacker_
```bash
samdump2 sys sam
```

__samdump2 tool__
- This tool is designed to dump Windows password hashes from a SAM file, _using the syskey bootkey from the system hive_.
- Recovers the syskey bootkey from a Windows system hive.
- __Syskey__ is a Windows feature that _adds an additional encryption layer to the password hashes_ stored in the SAM database.

__Mimikatz__
```powershell
sekurlsa:logonpasswords
```

__Cracking__
```hash
hashcat -m 1000 -a 0 hashes.txt $wordlist -O
```


---
### Tools
#### Password Cracking
##### Hashcat
- `m` : hash type
	- 0: MD5
	- 1000: NT hash
	- 
- `a` : attack type
	- 0 | Straight
	- 1 | Combination
	- 3 | Brute-force
	- 6 | Hybrid Wordlist + Mask
	- 7 | Hybrid Mask + Wordlist
	- 9 | Association
- _cracked hashes location:_ ~/.hashcat/hashcat.potfile

__Dictionary__
```bash
hashcat -m 0 -a 0 $hash $dictionary
```


__Rule Based__
```bash
hashcat -m 0 -a 0 -r /usr/share/doc/hashcat/rules/best64.rule $hash $wordlist
```

__Mask__
```bash
hashcat -m 0 -a 3 $hash ?u?a?a?d
```

__Mask - increment__
```bash
hashcat -m 0 -a 3 $hash --increment-min=3 --increment-max=5 ?u?a?a?d
```

__Combinator__

```bash


```













