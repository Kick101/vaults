<center><h3>LLMNR/NBT-NS/mDNS Poisoning</h3></center>

https://www.cynet.com/attack-techniques-hands-on/llmnr-nbt-ns-poisoning-and-credential-access-using-responder/

__Name Resolution (NR)__ It is a series of procedures conducted by a machine to retrieve a host's IP address by it's ___hostname___.

__Windows machine Procedure__
- Hostname's IP address is required
- The local hostfile is checked
- If no records are found then local DNS cache is checked
- If that also fails then a query will be sent to DNS server
- Even then if it fails then the machine will send a ___multicast query___ for the IP

__Name Resolution protocols__
- NBT-NS (NetBIOS Name Service) [old]
- LLMNR (Link-Local Multicast Name Resolution) [Supports NBT-NS]
- mDNS (multicast DNS) [Initially implemented by Linux]

#### Vulnerability
- The above protocols broadcast the query to the entire intranet for the IP 
- But doesn't verify the integrity of the responses
- So the attacker can exploit this by listening for queries and _spoof_ responses as a file server - that we're the host that victim is looking for

__Common abuse cases__
- Mistyping by the victim
- Misconfiguration of DNS either on the DNS side or victim's side
- WPAD protocol (Web Proxy Auto-Discovery | Windows Proxy Auto Detection) 
	- ###### Its legitimate use is to  automatically detect a network proxy used for connecting to the internet in corporate environments.
	- If a web browser is set to auto detect proxy settings, it'll use WPAD protocol to discover the URL of a _proxy configuration file._
	- To discover this URL, WPAD will iterate through potential URLs and hostnames - with each false attempt expose itself to spoofing
- Google Chrome
	- when text is typed in search bar it tires to resolve a hostname by sending DNS query
	- To prevent DNS hijacking, Chrome will try to resolve randomized hostnames on start-up to make sure they do not resolve - guaranteeing multicast NR action

##### Exploitation

- Attacker must be in the LAN and start the listener
- Use _responder_ from _impackets_: `# responder -I eth0 -rdw`
- `r` - Enable answers for netbios wredir suffix queries
- `d` - Enable answers for DHCP broadcast requests. This option will inject a WPAD server in the DHCP response.
- `w` - Start the WPAD rogue proxy server
- Once a multicast request is made, we spoof our response as the file server and grab the IP, hostname and NTLMv2 hash of the victim

<hr>
<center><h3>SMB Relay with NTLMv2 hash</h3></center>

- https://tcm-sec.com/smb-relay-attacks-gift-that-keeps-on-giving/
- https://www.sans.org/blog/smb-relay-demystified-and-ntlmv2-pwnage-with-python/
- https://notsosecure.com/pwning-with-responder-a-pentesters-guide
<br>

__Conditions for this to work__
- Relaying creds must be admin on victim machine 
- SMB singing is disabled or not required
<br>

__Discovering hosts with SMB signing disabled__
`# nmap --script=smb2-security-mode.nse -p445 192.168.69.0/24`
- Then start responder with http and SMB disabled in _/etc/responder.conf_ 

__Starting Relaying services__
`# ntlmrelayx.py -tf targets.txt -smb2support`
`# ntlmrelayx.py -tf targets.txt -smb2support -i` [SMB Interactive mode]
`# nc 127.0.0.1 11000` [Opens 11000 port on 127.0.0.1]
<br>


>we cannot “pass” NTLMv2 hashes around the network. Rather, we can “relay” them utilizing our MiTM position and a tool such as ntlmrelayx, which can dump NTLM hashes that can be passed to other machines or crack them offline.
<hr>
<center><h3>Gaining Shell Access</h3></center>

__Tools__
- psexec [msf] | psexec.py / smbexec.py / wmiexec.py [impackets]   

__Syntax__
`# psexec.py MARVEL.local/bbanner:"I'm always Angry"@192.168.69.102`

__Todo__
- Find what antivirus is running and disable it
- Gain shell with less noise

<hr>

<center><h3>IPv6 Attacks</h3></center>

https://blog.fox-it.com/2018/01/11/mitm6-compromising-ipv4-networks-via-ipv6/


##### Vulnerability
- Actual IPv6 addresses are auto-assigned by the hosts themselves and do not need to be configured by a DHCP server, this gives us the opportunity to set the attackers IP as the default IPv6 DNS server for the victims.
- Replies to DHCPv6 requests sent by clients in the network with a DHCPv6 reply containing the __attacker’s IP as DNS server.__

__How this works__
- Selectively spoofs hosts (the domain which is filtered on can be specified when running mitm6)
- Next step is to get clients to connect to the attacker machine instead of the legitimate servers
- Which is why we are spoofing URLs in the internal domain

__MS16-077 (Mitigation)__
- The location of the WPAD file is no longer requested via broadcast protocols, but only via DNS.
- Authentication does not occur automatically anymore even if this is requested by the server.

__Exploiting WPAD post MS16-077__
- The first protection, where WPAD is only requested via DNS, can be easily bypassed with mitm6. As soon as the victim machine has set the attacker as IPv6 DNS server, it will start querying for the WPAD configuration of the network. Since these DNS queries are sent to the attacker, it can just reply with its own IP address.
- To bypass the second protection, where credentials are no longer provided by default. When the victim requests a WPAD file we provide it with a valid WPAD file where the attacker’s machine is set as a proxy. When the victim now runs any application that uses the Windows API to connect to the internet or simply starts browsing the web, it will use the attackers machine as a proxy.


##### Exploitation
-  Starts replying to DHCPv6 requests and afterwards to DNS queries requesting names in the internal network
`# mitm6 -i eth1 -d marvel.local`
- Run _ntlmrelayx.py_ for serving WPAD file and relaying authentication
`# ntlmrelayx.py -6 -t ldaps://domain-ip -wh wpad.domain.local -l domain_info-loot`


<hr>

### Resources
TryHackMe
- ActiveDirectory
- Wreath